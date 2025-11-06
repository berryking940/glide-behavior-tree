ROOT
└── Selector (Glide Root)
    ├── Sequence (Start Glide)
    │   ├── Condition: IsInAir
    │   ├── Condition: AboveMinHeight
    │   ├── Condition: HasStamina(minStartStamina)
    │   ├── Condition: Input_IsGlidePressed
    │   ├── Action: EnterGlide (play anim, set flags, apply initial velocity)
    │   └── Action: StartGlideLoop (set glide state)
    │
    ├── Sequence (Glide Loop)  <-- 반복(동안) 노드 / 우선 실행되는 플로우
    │   ├── Condition: IsGlidingFlag TRUE
    │   ├── Selector
    │   │   ├── Sequence (Glide Continue)
    │   │   │   ├── Condition: NotCollidedSurface
    │   │   │   ├── Condition: HasStamina(minSustainStamina)
    │   │   │   ├── Action: ApplyGlidePhysics (gravity mod, forward lift, air control)
    │   │   │   ├── Action: DrainStamina (rate * dt)
    │   │   │   ├── Action: HandleInputsDuringGlide (steer, accelerate, dive)
    │   │   │   └── Action: SpawnGlideVFX/SFX
    │   │   │
    │   │   └── Sequence (Glide Exit Conditions)
    │   │       ├── Selector
    │   │       │   ├── Condition: CollidedSurface
    │   │       │   ├── Condition: StaminaDepleted
    │   │       │   └── Condition: Input_GlideReleased
    │   │       └── Action: ExitGlide (play land anim or transition, unset flags)
    │   │
    │   └── Action: UpdateGlideState (blackboard updates)
    │
    └── Fallback (NotGliding)
        ├── Condition: IsGlidingFlag FALSE
        └── Action: Idle/NormalAirPhysics
