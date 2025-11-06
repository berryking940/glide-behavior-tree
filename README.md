# glide-behavior-tree
Genshin-style gliding system behavior tree visualization
# 원신 활공 시스템 Behavior Tree

```mermaid
flowchart TD
    A[ROOT] --> B{Glide Root (Selector)}
    B --> C[Sequence: Start Glide]
    C --> C1[Condition: IsInAir]
    C --> C2[Condition: AboveMinHeight]
    C --> C3[Condition: HasStamina(minStartStamina)]
    C --> C4[Condition: Input_IsGlidePressed]
    C --> C5[Action: EnterGlide (anim, set flags)]
    C --> C6[Action: StartGlideLoop (state init)]

    B --> D[Sequence: Glide Loop]
    D --> D1[Condition: IsGlidingFlag TRUE]
    D --> D2{Selector: Glide Logic}

    D2 --> E[Sequence: Glide Continue]
    E --> E1[Condition: NotCollidedSurface]
    E --> E2[Condition: HasStamina(minSustainStamina)]
    E --> E3[Action: ApplyGlidePhysics]
    E --> E4[Action: DrainStamina(rate * dt)]
    E --> E5[Action: HandleInputsDuringGlide]
    E --> E6[Action: SpawnGlideVFX/SFX]

    D2 --> F[Sequence: Glide Exit Conditions]
    F --> F1{Selector: Exit Reason}
    F1 --> F11[Condition: CollidedSurface]
    F1 --> F12[Condition: StaminaDepleted]
    F1 --> F13[Condition: Input_GlideReleased]
    F --> F2[Action: ExitGlide (anim, unset flags)]

    D --> D3[Action: UpdateGlideState]

    B --> G[Fallback: NotGliding]
    G --> G1[Condition: IsGlidingFlag FALSE]
    G --> G2[Action: Idle/NormalAirPhysics]
```

