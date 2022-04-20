# Brief
@Author: Muru Chen

The finite state machine (FSM) is the parent class of train FSM. It includes entering state, updating state, handling events and exisiting state.

# Dependency
- global `Assets\Scripts\Train\FSM.cs`

``` c#
namespace FSM.State{
    using DicFSM = Dictionary<int, State>;
    abstract public class State
    {
        public State(StateMachine fsm, int id, string parentId = " ")
        {
            FSM = fsm; // The overall state machine that contains this state
            ID = id; // The state id
            ParentClassId = parentId; // The parent class that contains the fsm
        }
        public StateMachine FSM { get; set; }
        public string ParentClassId { get; set; }
        private int ID;
        virtual public int StateId {
            get 
            {
                return ID;
            }
        }
        // Implement some functions when enter a certain state
        public virtual void OnEnter() { Debug.Log("on enter: "+ ID); }
        // Keeps running when the FSM is at this state
        public virtual void OnUpdate() {}
        // Receive the signal and handle special events
        public virtual void OnEvent(string eventName, StateData data = null) { Debug.Log("on event: "+ eventName);}
        // Implement some functions when quit the state.
        public virtual void OnLeave() {}
    }

    public class StateData
    {
        public string strData;
        public Vector3 vecData;
        public int intData;
        // .....
        // Can add more kinds of variables
    }

    public class StateMachine
    {
        public StateMachine(DicFSM stateList, GameObject owner)
        {
            StateDic = stateList;
            Owner = owner;
        }

        public DicFSM StateDic { get; set; }
        public int CurrentStateId { get; set; }
        public bool IsStop { get; set; }
        public GameObject Owner { get; set; }

        // change the state.
        // It will call OnLeave() for current state
        // It will call OnEnter() for target state
        public void ChangeState(int stateId)
        {
            if (StateDic.ContainsKey(CurrentStateId))
            {
                State CurrentState = StateDic[CurrentStateId];
                if (CurrentState != null)
                {
                    CurrentState.OnLeave();
                }
            }
            if (StateDic.ContainsKey(stateId))
            {
                State nextState = StateDic[stateId];
                CurrentStateId = stateId;
                if (nextState != null)
                {
                    nextState.OnEnter();
                }
            }
        }

        // check if the fsm is in a certain state. Return true if the fsm is in this state
        public bool IsInState(int stateId)
        {
            if (CurrentStateId == stateId)
            {
                return true;
            }
            return false;
        }

        // Pass the eventname and the data for processing the event
        // Call OnEvent for current state.
        public void ProcessLocalEvent(string eventName, StateData data = null)
        {
            State curState = StateDic[CurrentStateId];
            if (curState != null)
            {
                curState.OnEvent(eventName, data);
            }
        }

        // Call this function in an Update()/FixedUpdate()
        public void RunFSM()
        {
            if (!StateDic.ContainsKey(CurrentStateId))
            {
                return;
            }
            State curState = StateDic[CurrentStateId];
            if (curState != null && !IsStop)
            {
                curState.OnUpdate();
            }
        }

        // Pause the state machine. The state machine will not run RunFSM()
        public void StopFSM(bool isStop_)
        {
            IsStop = isStop_;
        }
    }
}

```

# Example (see TrainMgr.c)

``` c#
    void InitializeStateMachine()
    {
        // Create new fsm
        stateMachine = new StateMachine(StateDic, train);

        // Create four state for each station
        for (int i = 1; i <= 4; ++i)
        {
            State state = (State)System.Activator.CreateInstance(Type.GetType("Window" + i.ToString()),
                                                                 stateMachine, i, id);
            StateDic.Add(i, state);
        }
    }
```