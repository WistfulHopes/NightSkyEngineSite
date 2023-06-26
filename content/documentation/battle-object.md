---
title: "Battle Object"
---

This page will document all blueprint accessible properties and functions of Battle Objects. Battle Objects are any gameplay-affecting object (such as projectiles), and is the base class for Player Objects.

## Variables

int32 PosX: The internal X position; left and right.

int32 PosY: The internal Y position; up and down.

int32 PosZ: The internal Z position; forward and back.

int32 SpeedX: The internal X speed.

int32 SpeedY: The internal Y speed.

int32 SpeedZ: The internal Z speed.

int32 SpeedXRate: Sets the percentage of SpeedX used.

int32 SpeedXRatePerFrame: A percentage multiplied against SpeedX every frame.

int32 SpeedYRate: Sets the percentage of SpeedY used.

int32 SpeedYRatePerFrame: A percentage multiplied against SpeedY every frame.

int32 SpeedZRate: Sets the percentage of SpeedZ used.

int32 SpeedZRatePerFrame: A percentage multiplied against SpeedZ every frame.

int32 Gravity: The internal gravity.

int32 Inertia: Inertia adds to the position every frame, but also decays every frame until it reaches zero.

int32 GroundHeight: The minimum Y position before the object is considered grounded.

EObjDir Direction: The current direction of the object (facing left or right).

FHitDataCommon HitCommon: Common data for attacks. These values will be used for blocking, normal hit, and counter hit. Values that are set to -1 will be replaced by a default value depending on attack level.

FHitData NormalHit: Hit data for normal hit. Values that are set to -1 will be replaced by a default value depending on attack level.

FHitData CounterHit: Hit data for counter hit. Values that are set to -1 will be replaced by the normal hit's value.

int32 ActionRegX (where X is a number from 1 to 8): Per-action registers. These registers are reset upon player state change. Shared between the player and its child objects.

int32 ObjectRegX (where X is a number from 1 to 8): Per-object registers. These registers are reset upon state change or object destruction. Not shared between the player and its child objects.

int32 ActionTime: How long the current state has been ongoing.

bool GotoLabelActive: If true, attempt to jump to the label set by the GotoLabel() function.

int32 AnimFrame: The current animation frame.

int32 BlendAnimFrame: The current blend animation frame.

float FrameBlendPosition: Used to blend between AnimFrame and BlendAnimFrame in the Anim Blueprint. Set by comparing the current cel time with the max cel time.

int32 CelIndex: The index of the current cel in blueprint.

int32 TimeUntilNextCel: How long until the next cel activates.

int32 MaxCelTime: Max duration of the cel.

FLinearColor MulColor: Internal representation of the MulColor material parameter. Intended to be multiplied against the final color.

FLinearColor AddColor: Internal representation of the AddColor material parameter. Intended to be added to the final color.

FLinearColor MulFadeColor: Depending on MulFadeSpeed, MulColor will fade to this value.

FLinearColor AddFadeColor: Depending on AddFadeSpeed, AddColor will fade to this value.

float MulFadeSpeed: MulColor will lerp to MulFadeColor every frame by this value.

float AddFadeSpeed: AddColor will lerp to AddFadeColor every frame by this value.

FVector ScaleForLink: Linked objects will be scaled to this value.

APlayerObject* Player: A pointer to self as a Player Object. If this object is not a player, it will point to the owning Player Object.

ABattleObject* AttackTarget: The most recently attacked object.

float ScreenSpaceDepthOffset: The material parameter used for depth offset.

float OrthoBlendActive: The material parameter used for orthographic blending.

## Functions

void InitEventHandler(EEventType EventType, FName FuncName): Initializes an event handler of the event type. When the given event fires, the given function will also activate if it exists in the current state.

void RemoveEventHandler(EEventType EventType): Removes the currently active event handler of the event type.

FString GetCelName(): Gets the current cel name.

FString GetAnimName(): Gets the current animation name.

FString GetBlendAnimName(): Gets the blend animation name.

FString GetLabelName(): Gets the name of the label being jumped to.

void SetCelName(FString InName): Sets the current cel name.

void SetBlendCelName(FString InName): Sets the blend cel name.

void GotoLabel(FString InName, bool ResetState): Attempts to jump to the given label in the state. If ResetState is true, the state will be re-entered, therefore also calling the Init function again.

void SetTimeUntilNextCel(int32 Time): Sets the time until the cel ends. 

void AddPosXWithDir(int InPosX): Adds X position with respect to current direction. For example, if InPosX is 50000 and the object is facing right, it will add 50000 to PosX. However, the object is facing left, it will subtract 50000 from PosX.

void SetSpeedXRaw(int InSpeedX): Sets Speed X with no regard to direction. This will act as if facing right, no matter what.

void AddSpeedXRaw(int InSpeedX): Adds Speed X with no regard to direction. This will act as if facing right, no matter what.

int32 CalculateDistanceBetweenPoints(EDistanceType Type, EObjType Obj1, EPosType Pos1, EObjType Obj2, EPosType Pos2): Calculates the distance between two points. The method depends on the distance type.

void SetFacing(EObjDir NewDir): Directly sets direction.

void FlipObject(): Flips the object.

void FaceOpponent(): Makes the object face the opponent.

bool CheckIsGrounded(): Checks if PosY is less than or equal to ground height.

void EnableHit(bool Enabled): Enables hitting the opponent.

void SetAttacking(bool Attacking): Sets attacking state. While this is true, you can be counter hit, but you can hit the opponent and chain cancel.

void EnableFlip(bool Enabled): Enables facing the opponent by default.

void EnableInertia(): Allows inertia being added to position.

void DisableInertia(): Disallows inertia being added to position.

void HaltMomentum(): Stops all movement, including gravity.

void SetPushCollisionActive(bool Active): Sets push collision being active.

void SetPushWidthExtend(int32 Extend): Adds Extend to push width.

void CreateCommonParticle(FString Name, EPosType PosType, FVector Offset, FRotator Rotation): Creates a common particle.

void CreateCharaParticle(FString Name, EPosType PosType, FVector Offset, FRotator Rotation): Creates a character particle.

void LinkCommonParticle(FString Name): Creates a common particle and attaches it to this object. Cannot be used with player objects.

void LinkCharaParticle(FString Name): Creates a character particle and attaches it to this object. Cannot be used with player objects.

void LinkCommonFlipbook(FString Name): Creates a common flipbook and attaches it to this object. Cannot be used with player objects.

void LinkCharaFlipbook(FString Name): Creates a character flipbook and attaches it to this object. Cannot be used with player objects.

void PlayCommonSound(FString Name): Plays a common sound.

void PlayCharaSound(FString Name): Plays a character sound.

void AttachToSocketOfObject(FString InSocketName, FVector Offset, EObjType ObjType): Attaches object to a skeletal socket of another object. Visual only.

void DetachFromSocket: Detaches object from skeletal socket.

void SetObjectID(int InObjectID): Sets the object ID. Used to prevent characters from entering a state that spawns an object with this ID while this object is active.

ABattleObject* GetBattleObject(EObjType Type): Gets an object by type.

ABattleObject* AddCommonBattleObject(FString InStateName, int32 PosXOffset, int32 PosYOffset, EPosType PosType): Adds a common battle object to the game.

ABattleObject* AddBattleObject(FString InStateName, int32 PosXOffset, int32 PosYOffset, EPosType PosType): Adds a character battle object to the game.

void EnableDeactivateIfBeyondBounds(bool Enable): Enables deactivating the object if it exits the stage bounds.

void EnableDeactivateOnStateChange(bool Enable): Enables deactivating the object if the player changes state.

void EnableDeactivateOnReceiveHit(bool Enable): Enables deactivating the object if the player gets hit or blocks.

void DeactivateObject(): Cannot be used on player objects. Deactivates the object and returns it to the pool.