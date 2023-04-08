# snippets
Snippets

## Platforming
```cs
using UnityEngine;
using System;
 
/*
--------------------------------------------------------------------
    PlayerFeet.cs
    Attach this class to a GameObject with a BoxCollider2D set to Trigger 
    and position at the feet of the Player.
--------------------------------------------------------------------
*/
[Serializable]
public class PlayerFeet : MonoBehaviour {
    public LayerMask whatIsGround;
    private PlayerController player;
    void Start() {
        player = GetComponentInParent<PlayerController>();
    }
    private void OnTriggerEnter(Collider2D other) {
        if (other.transform.gameObject.layer == LayerMask.NameToLayer("Ground")) {
            player.SetGrounded(true);
        }
    }
    private void OnTriggerStay(Collider2D other) {
        if (other.transform.gameObject.layer == LayerMask.NameToLayer("Ground")) {
            player.SetGrounded(true);
        }
    }
    private void OnTriggerExit(Collider2D other) {
        if (other.transform.gameObject.layer == LayerMask.NameToLayer("Ground")) {
            player.SetGrounded(false);
        }
    }
}
 
/* 
--------------------------------------------------------------------
    PlayerController.cs
    Attach this class to a GameObject with a BoxCollider2D
--------------------------------------------------------------------    
*/
using UnityEngine;
using System;
 
[Serializable]
public class PlayerController : MonoBehaviour {
  // ------------------
  // Variables 
  // ------------------
  private float speed = 5.5f;
  private float gravity = 8f;
  private float jumpForce = 16f;
  private float jumpDuration = 0;
  private float jumpDurationMax = 10f;
  private bool isCrouching = false;
  private bool isJumping = false;
  private bool isMoving = false;
 
  // ------------------
  // Game Lifecycle
  // ------------------
  // LOOP
  protected void Update(){
    // Input for Jump
    if (isJumping == false && Input.GetKeyDown(KeyCode.Space)){
      Jump();
    }
    CheckJumpForApex();
    // Input for Run
    if (Input.GetKey(KeyCode.LeftArrow)){
      Move(speed, Vector2.left);
    }
    else if (Input.GetKey(KeyCode.RightArrow)){
      Move(speed, Vector2.right);
    } else {
      isMoving = false;
    }
    // Input for Crouch
    if (isJumping == false && Input.GetKeyDown(KeyCode.DownArrow)){
      Crouch();
    }
    // Gravity
    ApplyGravity();
  }
 
  // ------------------
  // Verbs
  // ------------------
  private void ApplyGravity() {
    if (isJumping == true){
        Move(gravity, Vector2.down);
    }
  }
  private void Move(float speed, Vector2 direction){
    isMoving = true;
    transform.Translate(direction*fixedDeltaTime);
  }
  private void Crouch(){
    isCrouching = true;
    Debug.Log("Crouching");
    // implement logic here
  }
  private void CheckJumpForApex(){
    if (jumpDuration <= 0) {
        jumpDuration = 0;
    }
  }
  private void Jump(){
    if (jumpDuration <= 0) return;
    while (jumpDuration > 0) {
        Move(jumpForce, Vector2.up);
    }
  }
  public void SetGrounded(bool jumpState) {
    isJumping = jumpState;
    if (isJumping == false){
        jumpDuration = jumpDurationMax;
    }
  }
}
```

## State Management
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
