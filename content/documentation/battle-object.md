---
title: "Battle Actor"
date: 2023-01-26T01:24:39-08:00
---

# UNDERGOING REWRITE

This page will document all blueprint accessible properties and functions of battle actors. Battle actors are any gameplay-affecting object that is not a player (such as projectiles), and is the base class for player characters.

## Variables

int StateValX (where X is a number from 1 to 8): A "free" value, to be modified per-state/object. Upon exiting the state or an object's destruction, this value will reset to zero.

int AnimTime: Its intended purpose is to be used with the IsOnFrame function to execute code on the correct frame of the current state. Increments every frame, but can also be manually set. Changing states automatically resets this property to -1.

int AnimBPTime: With standard 3D characters, this property sets the frame of the current animation. Increments every frame, but can also be manually set. Changing states automatically resets this property to -1. Unused on 2D or ArcSystemWorks-style characters.

bool DefaultCommonAction: Some actions, such as dashing and air dashing, can have some parts of their default behavior overridden if this value is set to false. Changing states automatically resets this property to true.

APlayerCharacter* Player: The current player character. If the calling object is not a player, it will be the owning player. If the calling object is a player, it will be a reference to the subclass.

UMaterial* BoxMaterial: Only used for rendering collision in development or debug builds. In shipping builds, collision cannot be rendered.

FString CelName: Used to grab the correct animation and collision data. On 2D characters, it will also grab the sprite based on the last 2 digits. The cel name must end with _XX, where XX is the sprite "number".

UCollisionData* CollisionData: The collision data asset. 

UNiagaraComponent* LinkedParticle: For non-player actors only. Sets a particle that will move with the object.

## Functions

int GetInternalValue(EInternalValue InternalValue, EObjType ObjType = OBJ_Self): This function gets a value not directly exposed to blueprints. It can grab from itself, the parent player, or the enemy player.

bool IsOnFrame(int Frame): Checks if AnimTime is equal to Frame. Use with a Branch node to execute code on the correct AnimTime.

bool IsStopped(): Checks if the object is stopped by super freeze, hitstop, or throw.

void SetCelName(FString InCelName): Sets the CelName variable.

void SetHitEffectName(FString InHitEffectName): For custom hit effects, sets the name of the hit effect to be called.

void SetPosX(int InPosX): Sets x position.

void SetPosY(int InPosY): Sets y position.

void AddPosX(int InPosX): Adds to the current x position based on the current direction.

void AddPosXRaw(int InPosX): Adds to the current x position with no regard for the current direction.

void AddPosY(int InPosY): adds to the current y position.

void SetSpeedX(int InSpeedX): Sets x speed.

void SetSpeedY(int InSpeedY): Sets y speed.

void AddSpeedX(int InSpeedX): Adds to the current x speed.

void AddSpeedY(int InSpeedY): Adds to the current y speed.

void SetSpeedXPercent(int32 Percent): Multiplies the current x speed by this percent.

void SetSpeedXPercentPerFrame(int32 Percent): Every frame, the current x speed will be multiplied by this percent. Reset on state change.

void SetGravity(int InGravity): Sets gravity.

void SetInertia(int InInertia): Sets inertia. Inertia is a secondary speed value that has friction. In other words, it adds to position every frame in addition to speed, but decreases afterwards.

void ClearInertia(): Sets inertia to zero.

void EnableInertia(): Enables inertia.

void DisableInertia(): Disables inertia.

void HaltMomentum(): Stops all momentum, including speed, gravity, and inertia.

void SetPushWidthExpand(int Expand): The default pushbox width will be increased by the number Expand.

void SetFacing(bool NewFacingRight): If NewFacingRight is true, the object will face right; otherwise, the object will face left.

void FlipCharacter(): Flips the character's direction.

void EnableFlip(bool Enabled): Enables automatic direction flipping.

void EnableHit(bool Enabled): Enables hitboxes to hit the opponent's hurtboxes. If the function SetAttacking was not used to set attacking to true, this will do nothing.

void SetPushCollisionActive(bool Active): Toggles push collision.

void SetAttacking(bool Attacking): Enables attacking. While this is true, the object is in counter hit state, but can hit the opponent and chain cancel.

void SetHeadAttribute(bool HeadAttribute): Enables the head attribute on the current move. For use with attacks you want to make anti-airable. 

void SetProjectileAttribute(bool ProjectileAttribute): Enables the projectile attribute on the current move. For use with attacks that projectile invulnerability should ignore. 

void SetHitEffect(FHitEffect InHitEffect): Sets effects on hit for normal hit.

void SetCounterHitEffect(FHitEffect InHitEffect): Sets effects on hit for counter hit.

void CreateCommonParticle(FString Name, EPosType PosType, FVector Offset = FVector::ZeroVector, FRotator Rotation = FRotator::ZeroRotator): Creates a common particle.

void CreateCharaParticle(FString Name, EPosType PosType, FVector Offset = FVector::ZeroVector, FRotator Rotation = FRotator::ZeroRotator): Creates a particle specific to the current character.

void LinkCharaParticle(FString Name): Creates a character particle and attaches it to the current object. Do not use with player objects.

void PlayCommonSound(FString Name): Plays a common sound.

void PlayCharaSound(FString Name): Plays a sound specific to the current character.

void AttachToSocketOfObject(FString InSocketName, FVector Offset, EObjType ObjType): Attaches self to a mesh socket of the given object.

void DetachFromSocket(): Detaches from the socket given in AttachToSocketOfObject.

void PauseRoundTimer(bool Pause): Pauses the round timer.

void SetObjectID(int InObjectID): Sets the current object ID. Used in conjunction with a player character state's ObjectID. For example, if you attempt to enter a state while a fireball is on screen, and the fireball's ObjectID is equal to the state's ObjectID, the state cannot be entered.

ABattleActor* GetBattleActor(EObjType Type): Gets the specified battle actor if it exists, returns nullptr otherwise.

void DeactivateIfBeyondBounds(): _Do not use on player characters._ If the object exits the screen bounds, it will be deactivated.

void DeactivateObject(): _Do not use on player characters._ Deactivates the object.

void CollisionView(): Enables viewing the current collision data for this object. Only usable in development or debug builds.