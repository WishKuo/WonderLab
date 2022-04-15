# Brief
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
            FSM = fsm;
            ID = id;
            ParentClassId = parentId;
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
        public virtual void OnEnter() { Debug.Log("on enter: "+ ID); }
        public virtual void OnUpdate() {}
        public virtual void OnEvent(string eventName, StateData data = null) { Debug.Log("on event: "+ eventName);}
        public virtual void OnLeave() {}
    }

    public class StateData
    {
        public string strData;
        public Vector3 vecData;
        public int intData;
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

        public bool IsInState(int stateId)
        {
            if (CurrentStateId == stateId)
            {
                return true;
            }
            return false;
        }

        public void ProcessLocalEvent(string eventName, StateData data = null)
        {
            State curState = StateDic[CurrentStateId];
            if (curState != null)
            {
                curState.OnEvent(eventName, data);
            }
        }

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

        public void StopFSM(bool isStop_)
        {
            IsStop = isStop_;
        }
    }
}

```
