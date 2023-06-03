---
title: "Player Character"
---

# UNDERGOING REWRITE

This page will document all blueprint accessible properties and functions exclusive to player characters. For more info, check the [battle actor documentation page]({{< ref "/documentation/battle-object" >}} ).

## Variables

bool FlipInputs: When true, inputs will be flipped from the direction currently facing. Set to false on state change.

int32 PlayerIndex: Gets the player index (P1 or P2).

int32 FWalkSpeed: Forward walk speed.

int32 BWalkSpeed: Back walk speed.

int32 FDashInitSpeed: Initial forward dash speed.

int32 FDashAccel: Forward dash acceleration.

int32 FDashMaxSpeed: Maximum forward dash speed.

int32 FDashFriction: Forward dash friction.

int32 BDashSpeed: Back dash speed.

int32 BDashHeight: Back dash height.

int32 BDashGravity: Back dash gravity.

int32 JumpHeight: Jump height.

int32 FJumpSpeed: Forward jump speed.

int32 BJumpSpeed: Back jump speed.

int32 JumpGravity: Jump gravity.

int32 SuperJumpHeight: Super jump height.

int32 SuperFJumpSpeed: Super forward jump speed.

int32 SuperBJumpSpeed: Super back jump speed.

int32 SuperJumpGravity: Super jump gravity.

int32 AirDashMinimumHeight: Minimum height for air dash.

int32 FAirDashSpeed: Forward air dash speed.

int32 BAirDashSpeed: Back air dash speed.

int32 FAirDashTime: A timer set upon the start of a forward air dash. If the airdash is cancelled into an attack, you will start falling after this timer expires.

int32 BAirDashTime: A timer set upon the start of a back air dash. If the airdash is cancelled into an attack, you will start falling after this timer expires.

int32 FAirDashNoAttackTime: How long until you can cancel a forward air dash into an attack.

int32 BAirDashNoAttackTime: How long until you can cancel a back air dash into an attack.

int32 AirJumpCount: Total air jump count.

int32 AirDashCount: Total air dash count.

int32 StandPushWidth: Stand pushbox width.

int32 StandPushHeight: Stand pushbox height.

int32 CrouchPushWidth: Crouch pushbox width.

int32 CrouchPushHeight: Crouchpushbox height.

int32 AirPushWidth: Air pushbox width.

int32 AirPushHeight: Air pushbox height.

int32 AirPushHeightLow: Air pushbox bottom.

int32 CloseNormalRange: For proximity normals, the range in which close proximity normals will activate.

int32 Health: Maximum health.

int32 ComboRate: In a combo, every hit after the first hit of a combo will be prorated by this number.

int32 ForwardWalkMeterGain: Amount of meter gained every frame when walking forward.

int32 ForwardJumpMeterGain: Amount of meter gained every frame when jumping forward.

int32 ForwardDashMeterGain: Amount of meter gained every frame when dashing forward.

int32 ForwardAirDashMeterGain: Amount of meter gained every frame when air dashing forward.

int32 MeterPercentOnHit: Percent of meter gained depending on damage dealt. 

int32 MeterPercentOnHitGuard: Percent of meter when your attack is blocked, depending on the damage value of the hit.

int32 MeterPercentOnReceivedHitGuard: Percent of meter gained when blocking an attack, depending on the damage value of the hit.

int32 MeterPercentOnReceiveHit: Percent of meter gained depending on received damage.

TArray<FString> ThrowLockCels: An array of frames that can be used when being thrown.

int32 PlayerValX (where X is a number from 1 to 8): These are values that are not touched by the engine, except when the round ends, at which point they are reset to zero. As a result, they are free to be used in blueprints for any purpose, such as a counter or a timer.

APlayerCharacter* Enemy: A read-only reference to the enemy player.

USubroutineData* CommonSubroutineData: A list of common subroutines.

USubroutineData* CharaSubroutineData: A list of character-specific subroutines.

UStateDataAsset* StateDataAsset: A list of character states.

UStateDataAsset* ObjectStateDataAsset: A list of object states.

TArray<UState*> ObjectStates: An array of object states.

TArray<FString> ObjectStateNames: An array of object state names.

UParticleData* CommonParticleData: A list of common particles.

UParticleData* ParticleData: A list of character-specific particles.

USoundData* CommonSoundData: A list of common sound effects.

USoundData* SoundData: A list of character-specific sound effects.

USoundData* VoiceData: A list of character-specific voice lines.

USequenceData* CommonSequenceData: A list of common level sequences.

USequenceData* SequenceData: A list of character-specific level sequences.

int32 ColorIndex: The palette to use.

## Functions

void AddState(FString Name, UState* State): Adds a state to the state machine. Only use when initializing character in the base blueprint!

AddSubroutine(FString Name, USubroutine* Subroutine): Checks if AnimTime is equal to Frame. Use with a Branch node to execute code on the correct AnimTime.

void CallSubroutine(FString Name): Calls subroutine.

void UseMeter(int Meter): Uses meter.

void AddMeter(int Meter): Adds meter.

void SetMeterCooldownTimer(int Timer): Sets the duration for which meter gain will be lowered by 90%.

void SetActionFlags(EActionFlags ActionFlag): Sets standing/crouching/jumping.

void JumpToState(FString NewName): Forcibly sets state, even if the current state is the same.

FString GetCurrentStateName(): Gets current state name.

int32 GetLoopCount(): Used in tandem with IncrementLoopCount(). Gets the number of loops. Use to determine when to break the loop.

void IncrementLoopCount(): At the end of a loop, use to increment the loop count.

bool CheckStateEnabled(EStateType StateType): Checks if the state type is enabled.

void EnableState(EEnableFlags NewEnableFlags): Enables all states of the given type.

void EnableAttacks(): Enables all attacks.

void DisableState(EEnableFlags NewEnableFlags): Disables all states of the given type.

void DisableGroundMovement(): Disables ground movement only.

void EnableAll(): Enables all states, besides teching.

void DisableAll(): Disables all states, besides teching.

bool CheckInputRaw(EInputFlags Input): Checks input flags (after side switch).

bool CheckInput(EInputCondition Input): Checks input condition.

bool CheckIsStunned(): Checks if currently stunned (hitstun, blockstun, untech).

void AddAirJump(int32 NewAirJump): Adds the given number of air jumps to the current air jump count. Resets upon landing.

void AddAirDash(int32 NewAirDash): Adds the given number of air dashes to the current air dash count. Resets upon landing.

void SetAirDashTimer(bool IsForward): Sets the internal air dash timer.

void AddChainCancelOption(FString Option): Adds a chain cancel option.

void AddWhiffCancelOption(FString Option): Adds a whiff cancel option. Note that whiff cancels are disabled by default, and must be enabled per state.

void EnableJumpCancel(bool Enable): Enables jump cancel on hit or block. Off by default.

void EnableFAirDashCancel(bool Enable): Enables forward airdash cancel on hit or block. Off by default.

void EnableBAirDashCancel(bool Enable): Enables backward airdash cancel on hit or block. Off by default.

void EnableChainCancel(bool Enable): Enables chain cancel. On by default.

void EnableWhiffCancel(bool Enable): Enables whiff cancel. Off by default.

void EnableSpecialCancel(bool Enable): Enables special cancel. Off by default.

void EnableSuperCancel(bool Enable): Enables super cancel. Off by default.

void SetDefaultLandingAction(bool Enable): Sets default landing action. If true, touching the ground will immediately jump to the JumpLanding state. If false, allows you to handle landing actions yourself. Default landing action resets after touching the ground.

void SetStrikeInvulnerable(bool Invulnerable): Sets strike invulnerable.

void SetThrowInvulnerable(bool Invulnerable): Sets throw invulnerable.

void SetProjectileInvulnerable(bool Invulnerable): Sets projectile attribute invulnerable.

void SetHeadInvulnerable(bool Invulnerable): Sets head attribute invulnerable.

void ForceEnableFarNormal(bool Enable): Forcibly enables far proximity normals, even if inside close proximity normal range.

void SetThrowActive(bool Active): Sets throw active.

void ThrowEnd(): Ends the throw, allowing the grabbed character to move freely.

void SetThrowRange(int32 InThrowRange): Sets throw range.

void SetThrowExeState(FString ExeState): Sets the state that is jumped to when a throw activates.

void SetThrowPosition(int32 ThrowPosX, int32 ThrowPosY): Sets throw grip position.

void SetThrowLockCel(int32 Index): Sets the cel from the ThrowLockCels array to use. 

void PlayVoice(FString Name): Plays the given voice line.

void PlayCommonLevelSequence(FString Name): Plays the given common level sequence.

void PlayLevelSequence(FString Name): Plays the given character-specific level sequence.

void StartSuperFreeze(int Duration): Sets super freeze for the given duration.

void BattleHudVisibility(bool Visible): Toggles HUD visibility.

void DisableLastInput(): Disables the last input.

ABattleActor* AddBattleActor(FString InStateName, int32 PosXOffset, int32 PosYOffset): Adds a child battle actor with the given object state, and returns the created actor.

void AddBattleActorToStorage(ABattleActor* InActor, int Index): Stores a pointer to a battle actor to grab later.

void EmptyStateMachine(): **Only call at the start of InitStateMachine, and nowhere else! Otherwise, the game will crash!**

void EditorUpdate(): Updates the state machine for use with editor development tools.