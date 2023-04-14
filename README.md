# Quickstart

:warning: No process is perfect.

## State Management 
### 1. Controllers (C#)
This is used for handling the <u>implementation</u> of the logic and communicating with other objects in the game.
```cs
// Controller variables are always private
[SerializeField] private float _speed;
[SerializeField] private float _health;
[SerializeField] private float _damage;

// Getters are used to access the variables from a PlayMaker FSM
// Getters are written on one line
public float GetSpeed(){ return speed; } // Gets speed
public float GetHealth(){ return speed; } // Gets health
public float GetDamage(){ return speed; } // Gets damage
// ...
```

### 2. FSMs (PlayMaker)
```
// PlayMaker variables are local variables used as duplicates of the variables in the Controller
FSM_PLAYER_SPEED = /* Reads from Player.GetSpeed() */
```


## General tips
<img src="dir.png" width="440" />
<img src="01.png" width="440" />
<img src="02.png" width="440" />
<img src="03.png" width="440" />

## WIP Assets
+ https://www.spriters-resource.com

## Toolchain


## Mechanics
BossAI.cs
+ [Tutorial](https://www.youtube.com/watch?v=X7VwAGvAOIw)
```cs

```

SpawnEffect.cs
```cs

```

LifeBar.cs
```cs

```

Shoot.cs
```cs

```

TakeDamage.cs
```cs

```

ToggleInventoryScreen.cs
```cs

```
