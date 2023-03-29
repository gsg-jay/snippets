# snippets
Snippets


```cs
public class StateController : MonoBehaviour {
  private State currentState;
  public AttackMeleeState attackMeleeState = new AttackMeleeState();
  public AttackGunState attackGunState = new AttackGunState();
  public ChaseState chaseState = new ChaseState();
  public ClinchState clinchState = new ClinchState();
  public DeadState deadState = new DeadState();
  public DodgeState dodgeState = new DodgeState();
  public HurtState hurtState = new HurtState();
  public IdleState idleState = new IdleState();
  public IntroState introState = new IntroState();
  public PatrolState patrolState = new PatrolState();
  public RetreatState retreatState = new RetreatState();
  public StaggerState staggerState = new StaggerState();
  public TauntState tauntState = new TauntState();
  public TransformationState transformationState = new TransformationState();

  protected void Start() {
    ChangeState(idleState);
  }
  protected void Update() {
    if (currentState != null) {
      currentState.Execute();
    }
  }
  public void ChangeState (State newState) {
    if (currentState != null) {
      currentState.Exit();
    }
    currentState = newState;
    currentState.Enter();
  }
}

[Serializable]
public abstract class State {
  public bool isInitialized;
  public UnityEvent OnEnter;
  public UnityEvent OnExecute;
  public UnityEvent OnExit;
  public virtual void Enter(StateController controller);
  public virtual void Execute(StateController controller);
  public virtual void Exit(StateController controller);
}

[Serializable]
public class AttackState : State {
  public override void Enter(StateController controller) {
    OnEnter?.Invoke();
    Debug.Log("AttackState Enter");
  }
  public override void Execute(StateController controller) {
    if (!isInitialized) return;
    Debug.Log("AttackState Execute");
  }
  public override void Exit(StateController controller) {
    OnExit?.Invoke();
    Debug.Log("AttackState Exit");
  }
}

```
