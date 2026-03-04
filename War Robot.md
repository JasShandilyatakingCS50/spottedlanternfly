# Vulnerabilities Offset Mapping

All offsets from `dump.cs` (ARM64). Format: Component, Return Type, Method Name, Parameters, Offset, RVA/VA.

---

## 🏃 Category: Speed

### Speed Hack (AnimatorStateInfo)
* **Component:** `UnityEngine.AnimatorStateInfo`
* **Method:** `float get_speedMultiplier(void* instance)`
* **Offset:** `0x4CE7640` | **RVA:** `0x4CE8240`

### Speed (MechStats)
* **Component:** `Game.Stats.MechStats`
* **Method:** `float get_Speed(void* instance)`
* **Offset:** `0x241E730` | **RVA:** `0x241F330`

### SpeedFactor (AnimMotion)
* **Component:** `Game.MechMotion.AnimMotion`
* **Method:** `float get_SpeedFactor(void* instance)`
* **Offset:** `0x1054D20` | **RVA:** `0x1055920`

### Speed Stacked Multiplier
* **Component:** `Game.Stats.MechStats`
* **Method:** `float get_SpeedStackedMult(void* instance)`
* **Offset:** `0xE970D0` | **RVA:** `0xE97CD0`

---

## 🚀 Category: No Gravity

### Gravity Scale (Rigidbody2D)
* **Component:** `UnityEngine.Rigidbody2D`
* **Method:** `float get_gravityScale(void* instance)`
* **Offset:** `0x4DB3AB0` | **RVA:** `0x4DB46B0`

---

## 🦘 Category: Jump Impulse

### AnimJumping — Jump Force
* **Component:** `AnimJumping` (TypeDefIndex: 46)
* **Fields (patch directly):**
    * `_jumpVertImpulse` — field offset `0x28` (float) — vertical jump force
    * `_jumpHorizImpulse` — field offset `0x2C` (float) — horizontal jump force
* **Methods:**
    * `void Jump()` — **Offset:** `0x86D080` | **RVA:** `0x86DC80`
    * `bool get_CanJump()` — **Offset:** `0x86D470` | **RVA:** `0x86E070`

> **Usage:** Patch `_jumpVertImpulse` field to a huge value (e.g., 9999) for super jump, or to 0 to disable jumping. Hook `get_CanJump` to return `false` to prevent jump entirely.

---

## ⏱️ Category: Skill (Ability) Cooldown & Duration

### Ability1 Cooldown (MechStats)
* **Component:** `Game.Stats.MechStats`
* **Method:** `float get_Ability1Cooldown(void* instance)`
* **Offset:** `0xE9B140` | **RVA:** `0xE9BD40`

### Ability2 Cooldown (MechStats)
* **Component:** `Game.Stats.MechStats`
* **Method:** `float get_Ability2Cooldown(void* instance)`
* **Offset:** `0xE96E50` | **RVA:** `0xE97A50`

### Ability Cooldown (AbilityState Charges)
* **Component:** `Game.Abilities.States.Charges.AbilityStateWithCooldownAndCharges`
* **Method:** `float get_Cooldown(void* instance)`
* **Offset:** `0xF47050` | **RVA:** `0xF47C50`

### Ability1 Duration (MechStats)
* **Component:** `Game.Stats.MechStats`
* **Method:** `float get_Ability1Duration(void* instance)`
* **Offset:** `0xE9B160` | **RVA:** `0xE9BD60`

### Ability2 Duration (MechStats)
* **Component:** `Game.Stats.MechStats`
* **Method:** `float get_Ability2Duration(void* instance)`
* **Offset:** `0xE96E90` | **RVA:** `0xE97A90`

---

## 🎮 Category: Skill State Machine (Stop / Skip / Reset)

All powered by `NetworkAbilityCustomizable` (Game.Abilities):

### Skip Skill — GotoNextState
* **Method:** `void GotoNextState(void* instance)`
* **Offset:** `0xF35F20` | **RVA:** `0xF36B20`

### Stop Skill — GotoPrevStage
* **Method:** `void GotoPrevStage(void* instance)`
* **Offset:** `0xF35FD0` | **RVA:** `0xF36BD0`

### Go To Arbitrary State — GotoState
* **Method:** `void GotoState(void* instance, int nextState)`
* **Offset:** `0xF36080` | **RVA:** `0xF36C80`

### ChangeState (private, for force-transitioning)
* **Method:** `void ChangeState(void* instance, int newState)`
* **Offset:** `0xF356D0` | **RVA:** `0xF362D0`

### SetPressedState (simulate button press / release)
* **Method:** `void SetPressedState(void* instance, bool isPressed)`
* **Offset:** `0xF37360` | **RVA:** `0xF37F60`

### CurrentStateIndex
* **Method:** `int get_CurrentStateIndex(void* instance)`
* **Offset:** `0xB0FAC0` | **RVA:** `0xB106C0`

### Reset Skill — Auto-repeat
> Call `GotoState(0)` or `GotoNextState()` on cooldown timer expiry (hook the cooldown getter to detect when it hits 0, then call GotoState).

### Force Disable — Force ability into deactivated state
Call on `AbilityState`:
* `void Activate()` — **Offset:** `0xF32FF0` | **RVA:** `0xF33BF0`
* `void Deactivate()` — **Offset:** `0xF33090` | **RVA:** `0xF33C90`
* `void Stop()` — **Offset:** `0xF33280` | **RVA:** `0xF33E80`

> **Force Disable:** Call `Deactivate()` then `Stop()` on the active state to force-cancel the ability.
> **Force Status Effects without Ability:** Call `Activate()` on a particular state index to trigger its associated AbilityComponents (which apply StatusEffects) without going through the normal flow.

---

## 🛡️ Category: Anti-BlockSkill / Anti-BlockModule

### AbilityLockedByPilot (NetworkAbilityCustomizable)
* **Method:** `bool get_AbilityLockedByPilot(void* instance)`
* **Offset:** `0xF37950` | **RVA:** `0xF38550`
* **Hook:** Return `false` → ability is never pilot-locked.

### AbilityLockedByStatusEffect (NetworkAbilityCustomizable)
* **Method:** `bool get_AbilityLockedByStatusEffect(void* instance)`
* **Offset:** `0xF37960` | **RVA:** `0xF38560`
* **Hook:** Return `false` → ability is never locked by EMP/Lockdown/Suppression.

### ModuleLockedByStatusEffect (MechStats)
* **Component:** `Game.Stats.MechStats`
* **Method:** `bool get_ModuleLockedByStatusEffect(void* instance)`
* **Offset:** `0xE9B260` | **RVA:** `0xE9BE60`
* **Hook:** Return `false` → module is never locked by status effects.


---

## 🔧 Category: Module Cooldown & Duration

### Module Cooldown Multiplier (EquipmentStats)
* **Component:** `Game.Stats.EquipmentStatsAggregationHelper`
* **Method:** `float get_ModuleCooldownMultiplier(void* instance)`
* **Offset:** `0xE971F0` | **RVA:** `0xE97DF0`

### Module Cooldown (MechStats)
* **Component:** `Game.Stats.MechStats`
* **Method:** `float get_ModuleCooldown(void* instance)`
* **Offset:** `0xE96F30` | **RVA:** `0xE97B30`

### Module Duration (MechStats)
* **Method:** `float get_ModuleDuration(void* instance)`
* **Offset:** `0xE96FF0` | **RVA:** `0xE97BF0`

---

## 🎯 Category: Weapon Hacks

### Auto Fire — Shot Interval Multiplier
* **Component:** `Game.Stats.EquipmentStatsAggregationHelper`
* **Method:** `float get_ShotIntervalMultiplier(void* instance)`
* **Offset:** `0x4CE3FD4` | **RVA:** `0x4CE7FD4`

### No Sleep Shots — Burst Interval (field patches)

**WeaponBurstController** (`Game.Weapon.WeaponBurstController`):
| Field | Offset | Type | Purpose |
|---|---|---|---|
| `_burstInterval` | `0x24` | float | Delay between bursts |
| `_burstLength` | `0x20` | int | Shots per burst |

**WeaponBurstShooting** (`WarRobots.Gameplay.Equipments.Proxy.WeaponBurstShooting`):
| Field | Offset | Type | Purpose |
|---|---|---|---|
| `_shotInterval` | `0x58` | float | Delay between individual shots |
| `_burstInterval` | `0x60` | float | Delay between bursts |
| `_burstLength` | `0x5C` | int | Shots per burst |

> Patch `_burstInterval` to 0 and increase `_burstLength` to remove burst delay.

### Weapon Range 1100m
* **Method:** `float get_RangeMultiplier(void* instance)`
* **Offset:** `0x4DA61F0` | **RVA:** `0x4DA6DF0`

### No Ammo Reload Lite
* **Method:** `float get_ReloadAccelerationMultiplier(void* instance)`
* **Offset:** `0xE97270` | **RVA:** `0xE97E70`

### No Recoil — Dispersion
* **Method:** `float get_DispersionDeltaTimeMultiplier(void* instance)`
* **Offset:** `0xE97090` | **RVA:** `0xE97C90`

---

## 🎥 Category: Player Mods

### FOV View
* **Component:** `UnityEngine.Camera`
* **Method:** `float get_fieldOfView(void* instance)`
* **Offset:** `0x4CF2CD0` | **RVA:** `0x4CF38D0`

### God Mode / Damage Resist
* **Component:** `Game.Stats.EquipmentStatsAggregationHelper`
* **Method:** `float get_DamageResistMultiplier(void* instance)`
* **Offset:** `0xE96FF0` | **RVA:** `0xE97BF0`

---

## 🛡️ Category: Anti-Status Effects (Anti-LockDown, Anti-Blind, Anti-EMP)

### StatusEffectTarget.HasEffect (main hook)
* **Component:** `Game.Match.StatusEffectTarget`
* **Method:** `bool HasEffect(void* instance, int effectKind)`
* **Offset:** `0xFD7410` | **RVA:** `0xFD8010`

**EffectKind Constants:**
| Effect | ID | Immune ID |
|---|---|---|
| Root (LockDown) | `2` | `3` |
| Suppression | `4` | `5` |
| Freeze | `44` | `45` |
| EMP | `86` | `87` |
| Blind | `156` | `157` |
| Slowdown | `162` | `163` |
| Stealth | `60` | — |
| ControlDumper | `34` | `35` |
| ControlWiper | `84` | — |
| CounterBlind | `254` | `255` |
| CounterSuppression | `240` | `241` |

> **Anti-LockDown:** Hook `HasEffect`, when `effectKind == 2`, return `false`.
> **Anti-Blind:** Hook `HasEffect`, when `effectKind == 156`, return `false`.
> **Anti-EMP:** Hook `HasEffect`, when `effectKind == 86`, return `false`.
> **Anti-BlockSkill:** Hook `HasEffect`, filter `effectKind` values that cause ability lock (2, 4, 86). Combined with `get_AbilityLockedByStatusEffect` returning `false`.
> **Anti-BlockModule:** Hook `get_ModuleLockedByStatusEffect` to return `false`.

### CanMove (AnimMotion) — Anti-LockDown alternative
* **Method:** `bool get_CanMove(void* instance)`
* **Offset:** `0x1054C90` | **RVA:** `0x1055890`
* **Hook:** Return `true` → always movable.

---

## 🤖 Category: Force Drone Status Effects

Use the same `HasEffect` hook at RVA `0xFD8010` but return `true` for desired `EffectKind` values to force drone effects as if active.

Relevant EffectKind values:
| Effect | ID |
|---|---|
| DroneLastStand | `232` |
| DroneSlowdown | `392` |
| DroneSlowdownImmune | `393` |

---

## 💫 Category: Force Ability Status Effects

Force the ability's status effects without activating the ability. Two approaches:

**Approach 1 — State Machine:** Call `AbilityState.Activate()` at RVA `0x4D8BB98` on a specific state to trigger its `AbilityComponent[]` (which apply StatusEffects) without using the full flow.
** 	protected AbilityComponent[] _components; // 0x28 **
**Approach 2 — HasEffect return `true`:** For specific EffectKind values:
| Effect | ID |
|---|---|
| PhaseShift | `42` |
| Stealth | `60` |
| LastStand | `6` |
| Berserk | `62` |
| EnergyShield | `74` |
| AbsorberShield | `76` |
| Nitro | `102` |
| Rage | `104` |
| Invisibility | `372` |

### Force Disable
Rapidly cycle `Deactivate()` (RVA `0xF33C90`) → `Stop()` (RVA `0xF33E80`) on the active state to interrupt and force-cancel. This effectively stacks interruption on abilities that have multi-phase states, causing forced deactivation.

---

## 🧭 Category: Teleportation

### TeleportationService.StartTransition — Beacon Teleport
* **Component:** `Game.Match.Services.TeleportationService`
* **Method:** `bool StartTransition(void* instance, void* actor, int targetObjectId)`
* **Offset:** `0xFFE270` | **RVA:** `0xFFEE70`

### TeleportationService.CancelTransition
* **Method:** `bool CancelTransition(void* instance, void* actor, int targetObjectId)`
* **Offset:** `0xFFDDE0` | **RVA:** `0xFFE9E0`

### HandleTeleportation (direct)
* **Component:** `Game.Match.Services.StaticTeleportManager`
* **Method:** `void HandleTeleportation(void* instance, void* actor, void* entry, int exitIndex, bool isLocal)`
* **Offset:** `0x4E28A9C` | **RVA:** `0x4E2CA9C`

### HandleTeleportationDebug (force any exit point)
* **Method:** `void HandleTeleportationDebug(void* instance, void* actor, void* exit, void* point)`
* **Offset:** `0xFFB690` | **RVA:** `0xFFC290`

**Teleport-to-Player Modes (1 choice):**

1. **Closest player:** Enumerate actors, find nearest → use its position.

All 3 ultimately call `Transform.set_position` to place the player at the target.

### Transform Position (for raw position override)
* **Component:** `UnityEngine.Transform`
* **Getter:** `Vector3 get_position()` — **Offset:** `0x4D3EF70` | **RVA:** `0x4D3FB70`
* **Setter:** `void set_position(Vector3)` — **Offset:** `0x4D3F7C0` | **RVA:** `0x4D403C0`

---

## 🕳️ Category: Underground Serve

Uses `Transform.set_position` (above) to set the player's Y coordinate to a negative offset below their current position (or below a teleported-to player's position).

**Implementation:** Hook `Transform.set_position` or call it directly:
1. Get current position via `get_position()` at RVA `0x4D3FB70`
2. Subtract desired depth from `Y` component (e.g., 40m to 5000m) DO NOT OVERWRITE The Y, INSTEAD JUST SUBSTRACT, OR IT WILL BREAK THE GAME.
3. Set with `set_position()` at RVA `0x4D403C0`

> The underground serve works by continuously forcing the Y position below the ground plane. The player walks normally but is rendered underground.

---




---

## 🔍 Category: Bonus Exploitable Methods

### Target Lock Speed
* **Method:** `float get_TargetLockSpeed(void* instance)`
* **Offset:** `0xE97010` | **RVA:** `0xE97C10`

### HP Mech
* **Method:** `float get_HpMech(void* instance)`
* **Offset:** `0xE971B0` | **RVA:** `0xE97DB0`

### HP Physical Shields
* **Method:** `float get_HpPhysicalShields(void* instance)`
* **Offset:** `0xE97170` | **RVA:** `0xE97D70`

### HP Energy Shields
* **Method:** `float get_HpEnergyShields(void* instance)`
* **Offset:** `0xE97170` | **RVA:** `0xE97D70`

### Additional Ability1/2 Charges
* `int get_AdditionalAbility1Charges()` — **RVA:** `0xE9BE00`
* `int get_AdditionalAbility2Charges()` — **RVA:** `0xE9BE20`

### Module Price (free modules)
* **Method:** `float get_ModulePrice(void* instance)`
* **Offset:** `0xE9B280` | **RVA:** `0xE9BE80`

### Turret Rotation Speed
* **Method:** `float get_TurretRotationSpeedStackedMult(void* instance)`
* **Offset:** `0xE970B0` | **RVA:** `0xE97CB0`

### Damage By Distance Multipliers
* `get_DamageByDistanceMinMultiplier` — **RVA:** `0xE97BB0`
* `get_DamageByDistanceMaxMultiplier` — **RVA:** `0xE97BB0`

### AntiStealth Radius Helper
* **Component:** `WarRobots.Gameplay.StatusEffects.AntiStealthEffectHelper`
* **Method:** `bool IsInAntiStealthRadius(void* shooter, void* target)`
* **Offset:** `0x7454280` | **RVA:** `0x7722880`

### CanBeAimed (bypass stealth on target)
* **Component:** `Game.Actors.DefenceActor`
* **Method:** `bool CanBeAimed(void* instance, void* shooter, bool checkStealthOnly)`
* **Offset:** `0x10563D0` | **RVA:** `0x1056FD0`
* **Hook:** Return `true` → target always aimable regardless of stealth.

### ShotInterval getter/setter (IShootingWeapon)
* **Getter:** `float get_ShotInterval()` — **RVA:** `0x90D0B0`
* **Setter:** `void set_ShotInterval(float)` — **RVA:** `0x90D690`

### ScheduleShots (force shoot)
* **Method:** `void ScheduleShots(void* instance, int count)` — **RVA:** `0x90CE70`

---


---

## 💀 Category: DamageController — HP / Heal / Death Manipulation

### DamageController (main health system)
* **Component:** `Game.Combat.DamageController` (TypeDefIndex: 7642)

| Property | Getter RVA | Setter RVA | Type |
|---|---|---|---|
| `Hp` | `0x103CD90` | `0x103D660` (private) | float |
| `MaxHp` | `0x103CDC0` | `0x103D690` | float |
| `HpNormalized` | `0x103CD70` | — | float |
| `HealLimit` | `0x103CD60` | `0x103D5A0` | float |
| `HealScale` | `0x9466A0` | `0x945AA0` | float |
| `DamageScale` | `0x4E87CC4` | `0x4E87CCC` | float |
| `IsDestructed` | `0x103CDA0` | — | bool |
| `IsRecoverable` | `0x7A2B90` | — | bool |
| `ArmorPoints` | `0x103CC90` | — | float |
| `IsShield` | `0x8794A0` | — | bool |
| `BlocksAoeDamage` | `0x103CCD0` | `0x103D510` | bool |

**Advanced HP hacks:**
* **Infinite HP:** Hook `get_Hp` to always return `get_MaxHp` value.
* **Anti-Death:** Hook `get_IsDestructed` to return `false` → mech can never be flagged as destroyed.
* **HealScale Boost:** Set `HealScale` to a massive multiplier → all incoming healing is amplified.
* **DamageScale Zero:** Set `DamageScale` to `0.0f` → incoming damage is multiplied by zero.
* **MaxHp Inflation:** Set `MaxHp` to an extreme value → massive health pool.
* **Remove Armor Bypass:** Hook `get_ArmorPoints` to return a huge value → armor always absorbs shots.
* **Anti-AoE:** Set `BlocksAoeDamage` to `true` on player → splash damage blocked.
* **No Heal Limit:** Set `HealLimit` to `get_MaxHp` value → no cap on healing.

---


---

## 🏔️ Category: MechDynamics — Ground State / Altitude

### MechDynamics
* **Component:** `MechDynamics` (TypeDefIndex within Mech)

| Property | RVA | Type |
|---|---|---|
| `get_isGrounded` | `0x8794A0` | bool |
| `set_isGrounded` (private) | `0x879560` | bool |
| `get_isLanding` | `0x8794B0` | bool |
| `GetMechAltitude(out RaycastHit)` | `0x878C10` | float |
| `get_rigidbody` | `0x7A0DC0` | Rigidbody |

> **Permanent Float/Fly:** Hook `get_isGrounded` to return `false` + manipulate rigidbody velocity.
> **Forced Ground (anti-fly):** Hook `get_isGrounded` to return `true` always.
> **Altitude Spoof:** Hook `GetMechAltitude` to return arbitrary height values.
> **Rigidbody Access:** `get_rigidbody` gives access to Unity physics (velocity, angular velocity, mass, drag).

---

## 🎯 Category: Projectile Speed / Collision Manipulation

### IShootingWeapon — Projectile Speed
* **Component:** `Game.Weapon.ShootingWeapon` (IShootingWeapon interface)

| Property | Getter RVA | Setter RVA |
|---|---|---|
| `ProjectileSpeed` | `0x47698EC` | `0x47698FC` |
| `ProjectilesPerShot` | `0x4769974` | `0x476997C` |
| `AmmoConsumptionPerShot` | `0x4769D7C` | `0x4769D84` |
| `ammo` (current) | `0x4769B40` | — |
| `ammoCapacity` | `0x4769DE4` | — |
| `IsReloading` | `0x4769DEC` | — |
| `isShootingLockedByReload` | `0x4769E1C` | — |
| `maxRange` | `0x4769E3C` | `0x4769E44` |
| `IsLocked` | `0x47699EC` | — |

> **Instant Hit Projectile:** Set `ProjectileSpeed` to extreme value → bullets hit instantly.
> **Shotgun Mode:** Increase `ProjectilesPerShot` → more projectiles per shot.
> **Infinite Ammo:** Hook `get_ammo` to always return `get_ammoCapacity`. Or set `AmmoConsumptionPerShot` to `0`.
> **No Reload Wait:** Hook `get_IsReloading` to return `false` and `get_isShootingLockedByReload` to return `false`.
> **Extended Range (per-weapon):** Set `maxRange` directly per weapon instance.
> **Unlock Shooting:** Hook `get_IsLocked` to return `false` → weapon always enabled.

---

## 🎯 Category: Auto-Aim / Target Controller

### TargetController
* **Component:** `Game.Weapon.TargetController` (TypeDefIndex: 6976)
* **Interface:** `ITargetController` (TypeDefIndex: 6966)

### TargetAutoAimGroup
* **Component:** `Game.Weapon.TargetAutoAimGroup` (TypeDefIndex: 6974)
* Extends `TargetGroup` — defines which actors are auto-aim eligible.

### IAimingController
* **Interface:** `IAimingController` (TypeDefIndex: 6930)
* The Turret class (`Game.Weapon.Turret`, TypeDefIndex: 6951) implements `IAimingController` and controls weapon aiming.

> **Perfect Aim:** Hook the Turret's aiming update to always point toward the nearest target's position (combine with `Transform.get_position` on the target actor).
> **Anti-Aim (for self):** Corrupt the aiming data that enemies receive about your position.

---

## 🛡️ Category: EnergyShield Manipulation

### EnergyShieldOwner
* **Component:** `Game.Shield.EnergyShieldOwner` (TypeDefIndex: 6909)
* Manages energy shield instances on a mech.

### EnergyShieldService
* **Component:** `Game.Match.Services.EnergyShieldService` (TypeDefIndex: 10099)
* Central service managing all energy shields in the match.

### EnergyShieldSystemBase
* **Component:** `Game.Shield.EnergyShieldSystemBase` (TypeDefIndex: 6928)
* Implements `IHas<IHitController>` and `IHas<IHealthController>`.
* Energy shields have their OWN `DamageController` — hooking the shield's `DamageController.get_Hp` separately can grant invincible shields.

> **Infinite Shield:** Hook the energy shield's DamageController so `get_Hp` always returns `get_MaxHp`.
> **Shield Bypass (enemy):** Hook `get_IsShield` to return `false` for enemy shields → damage goes straight to hull.

---

## 🔄 Category: Spawn Manipulation

### SpawnService
* **Component:** `Game.SpawnService` (TypeDefIndex: 614)
* Handles player respawning, zone-based spawn restrictions, and spawn timing.

### IsTitanChargeEnoughForMech
* **Method (on SpawnService):** `bool IsTitanChargeEnoughForMech(int slotIndex, MechKind mechKind)`
* Hook to return `true` → deploy titans without enough charge.

> **Instant Respawn:** Manipulate spawn timer or hook spawn cooldown to 0.
> **Deploy Any Mech:** Bypass titan charge and slot restrictions.

---

## 🌐 Category: Miscellaneous Advanced Exploits

### Accuracy (EquipmentStats)
* **Component:** `Game.Stats.EquipmentStatsAggregationHelper`
* `get_AccuracyMultiplier` — increases weapon accuracy
* `get_AccuracyAdditional` — flat accuracy bonus

### Charge Rate (EquipmentStats)
* `get_ChargeRateMultiplier` — affects charge-based weapons (e.g., Gauss, Weber)

### Energy Refresh Rate (EquipmentStats)
* `get_EnergyRefreshRateMultiplier` — controls energy weapon regeneration speed

### Acceleration Time (EquipmentStats)
* `get_AccelerationTimeMultiplier` — affects time-to-max-speed for spinning weapons

### MechStats.StatType enum
Can be used with `AbilityMechStatElementComponent` to discover all possible stat modifications:
* Hooks the `_statType` field to redirect which stat an ability modifies.

### PressedStateMediator (ability button simulation)
* **Component:** `Game.Abilities.PressedStateMediator` (TypeDefIndex: 6704)
* `UpdateUiPressedState(bool)` — RVA: `0x4D8A108`
* `UpdateKeyPressedState(bool)` — RVA: `0x4D8A1C8`

> Programmatically simulate ability button press/release without UI interaction.

### Rigidbody (direct physics access via MechDynamics)
* Access via `get_rigidbody` (RVA `0x46D178C`) on MechDynamics.
* From Rigidbody: `set_velocity`, `set_mass`, `set_drag`, `set_angularVelocity`, `set_useGravity`.
* **Zero Mass:** Set mass to near-zero → mech slides on impacts and flies on explosions.
* **Custom Velocity:** Directly set velocity vector for teleport-like movement or speed hacks that bypass AnimMotion.

---
---

# 🔴 CLIENT → SERVER TRUST BOUNDARY ANALYSIS

> This section identifies which client-side modifications can trigger **server-side state changes** (i.e., affect other players), vs which are purely local/cosmetic.

---

## 🟢 Confirmed Client-Authoritative (Server Trusts the Client)

### 1. Ability State Dispatch — `AbilityStateChange` RPC
The client sends `AbilityStateChange` via `SendAbilityStateChange` (RVA `0x470EB4C`).

**RPC struct `AbilityStateChange` (ClientCommons.Game.Rpc):**
| Field | Offset | Type | Client-Controlled? |
|---|---|---|---|
| `AbilityType` | `0x0` | byte | ✅ |
| `Activated` | `0x1` | bool | ✅ |
| `Version` | `0x4` | int | ✅ |
| `ActivateTimeMilliSeconds` | `0x8` | int | ✅ |
| `ModuleSlotIndex` | `0xC` | byte? | ✅ |
| `ModuleIsActive` | `0xE` | bool | ✅ |
| `AbilitySourceKind` | `0xF` | byte? | ✅ |

> **Exploit — Module Spam:** The client tells the server "I activated my module at time X." By rapidly calling `SendAbilityStateChange` with `Activated=true` and manipulated `ActivateTimeMilliSeconds` (timestamped in the past to bypass cooldown), the server may apply the module effect multiple times. Modules that heal HP, apply shields, or deal damage to nearby enemies will have **server-side effects** because the server trusts the client's activation message.

> **Exploit — Ability Cooldown Bypass:** Forge `Version` and `ActivateTimeMilliSeconds` to reactivate abilities before their cooldown expires server-side.

### 2. Spawn Request — `SpawnMechRequest` RPC
**Struct:** `SpawnMechRequest`
| Field | Type | Client-Controlled? |
|---|---|---|
| `SpawnPointId` | int | ✅ |
| `PointsByDistance` | byte[] | ✅ |
| `ServerMechId` | string | ✅ |
| `DroneId` | string | ✅ |

> **Exploit — Spawn Point Manipulation:** Client chooses spawn point. Send invalid `SpawnPointId` or one belonging to another team's zone.
> **Exploit — Mech/Drone ID Spoofing:** Potentially deploy mechs or drones the player doesn't own by sending a different `ServerMechId`.

### 3. Beacon Zone Presence — `RemotePresenceInZoneChanged` RPC
**Struct:** `RemotePresenceInZoneChanged`
| Field | Type | Client-Controlled? |
|---|---|---|
| `Key` | string (zone) | ✅ |
| `IsIn` | bool | ✅ |
| `ViewId` | int | ✅ |

> **Exploit — Remote Beacon Capture:** Tell the server you are "in" a beacon zone (`IsIn=true`) from anywhere on the map. The server may credit beacon capture progress without the player physically being there.

### 4. Position / Motion — Client-Sent Position Updates
Position data flows from client to server via the network observer system. The `NetworkPositionUpdated` event (on turret/position observers) fires with `(ViewId, uint timestamp, Vector3 position)`.

> **Exploit — Teleportation (server-recognized):** The client's position updates are serialized to the server. Setting your mech's position arbitrarily via `Transform.set_position` will sync to the server's understanding of where you are. This is WHY teleport hacks work — the server trusts client position.
> **Exploit — Underground Serve (server-recognized):** Setting Y to extreme negative values syncs to server. Other players see you underground because the server accepted the position.

### 5. Damage Flag `_damageCalculatedOnServer`
* **Component:** `NetDamage`
* **Field:** `_damageCalculatedOnServer` at offset `0x70` (bool)

> **Exploit potential:** If this flag is `false`, damage calculations happen client-side and results are serialized to the server via `SerializeWillHpChangeDamageV2` (RVA `0x4E9BA44`). Patching this to `false` may cause the server to accept client-calculated damage values, enabling damage amplification that affects server-side HP.

---

## 🟡 Client-Side with Server-Side Side Effects

These modifications are processed locally but trigger network-synced behavior.

### 6. Ability State Machine → Server Effect Chain
Calling `GotoNextState()` or `GotoState()` on `NetworkAbilityCustomizable` triggers `DispatchNetworkAbility` (RVA `0x4D88F18`) which calls `SendAbilityStateChange` — so **ability state manipulation is automatically sent to the server**.

**Server-side cascade:**
- Skip/Stop/Reset skill → triggers `AbilityStateChange` RPC → server applies ability activation or deactivation effects
- Module abilities that heal, shield, or deal damage → the server grants the HP/shield/damage
- **This is why "module spam" hacks work** — each client-side `GotoState()` fires a network dispatch that the server processes

### 7. Status Effects via Ability Activation
`AbilityState.Activate()` triggers `AbilityComponent[]` which apply status effects. These effects are dispatched to the server via the ability RPC system.

> Force-activating stealth, phase shift, or last stand via the state machine sends the activation to the server, which then hides your mech from other players.

### 8. Shot Scheduling → Server Hit Registration
`ScheduleShots(int count)` on `IShootingWeapon` schedules shots that go through the normal projectile pipeline, which includes network hit registration. The server processes these shots.

> **Exploit:** Call `ScheduleShots` with a large count while modifying `_shotInterval` to 0 → rapid fire that the server registers as legitimate hits.

---

## 🔴 Server-Side Only (Client Cannot Modify)

These values are confirmed or likely server-authoritative:

| Feature | Why | Notes |
|---|---|---|
| **Silver/Gold/Currency** | Server database | Client only displays cached values |
| **Honor Points** | Calculated server-side | Based on server-tracked kill/damage/beacon events |
| **Inventory (Mechs/Weapons owned)** | Server database | SpawnMechRequest references `ServerMechId` — server validates ownership |
| **Match Results** | Server-calculated | Win/loss determined by server |
| **Player Level/XP** | Server database | — |
| **Titan Charge (bar)** | Partially server | Server sends `ServerTitanCharge` RPC to client; client display only |
| **HP of other players** | Server-calculated | NetDamage receives `RemoteUpdateHpV2` from server |
| **Enemy mech stats** | Server-dispatched | Balance data comes from server |

---

## ⚡ Highest-Impact Client→Server Exploits (Summary)

| Exploit | Mechanism | Server Impact |
|---|---|---|
| **Module Spam** | Forge rapid `AbilityStateChange` RPCs | Server applies module effects (heal/shield/damage) multiple times |
| **Ability Cooldown Bypass** | Manipulate `ActivateTimeMilliSeconds` | Server activates ability before cooldown expires |
| **Teleport / Underground** | Client `Transform.set_position` syncs | Server accepts new position; affects all players |
| **Remote Beacon Capture** | Forge `RemotePresenceInZoneChanged` | Server credits beacon capture without player presence |
| **Rapid Fire (shot spam)** | `ScheduleShots` + modified intervals | Server registers rapid hits as damage |
| **Client-Side Damage Calc** | Toggle `_damageCalculatedOnServer` flag | Server may accept inflated damage values |
| **Stealth/Phase Force** | Ability state machine → network dispatch | Server hides player from enemies |
| **Spawn Point Override** | Forge `SpawnMechRequest.SpawnPointId` | May spawn in enemy territory |

---
---

# 🛡️ STATUS EFFECT INJECTION — CAN YOU ADD REFLECTOR / COUNTERBLIND / COUNTERSUPPRESSION TO ANY BOT?

**Short answer:** Yes, partially. The client CAN inject effects locally. Whether the server accepts and broadcasts them depends on the injection vector.

---

## The StatusEffect Struct (fully constructable client-side)

```
StatusEffect {
    IReadOnlyDictionary<EffectParam, float> Values; // 0x0
    int    Id;              // 0x8
    float  EndTime;         // 0xC
    float  InitialDuration; // 0x10
    ViewId SourceViewId;    // 0x14
    ViewId TargetViewId;    // 0x18
    EffectKind Kind;        // 0x1C (the effect type — THIS is the key)
    bool   IsMine;          // 0x1E
    bool   IsInfinite;      // 0x1F
    bool   FromDrone;       // 0x20
}
```

**Target EffectKind values for injection:**
| Shield / Effect | EffectKind ID |
|---|---|
| Reflector (EnergyShield) | `74` |
| AbsorberShield | `76` |
| CounterBlind | `254` |
| CounterBlindImmune | `255` |
| CounterSuppression | `240` |
| CounterSuppressionImmune | `241` |
| PhaseShift | `42` |
| Stealth | `60` |
| LastStand | `6` |

---

## 3 Attack Vectors (ranked by server impact)

### Vector 1: `HasEffect` Hook (Client-Only, No Server Impact)
* **Method:** Hook `StatusEffectTarget.HasEffect(EffectKind)` to return `true` for desired effect IDs.
* **Server Impact:** ❌ NONE — purely client-side. Your mech *behaves* as if it has the effect locally (e.g., reflects damage in local calculations, shows shield VFX), but the server doesn't know.
* **Use case:** Visual-only; local anti-blind, anti-suppression.

### Vector 2: `StatusEffectsTarget.AddEffect(StatusEffect)` — Local Effect Injection
* **Component:** `Game.Match.StatusEffectsTarget` (TypeDefIndex: 7228)
* **Method:** `void AddEffect(StatusEffect effect)` — **RVA:** `0x4E11808`
* **StatusEffectShieldController** watches for `EffectAdded` events → if `Kind == EnergyShield (74)`, it creates an `EnergyShieldSource` and attaches it to the `EnergyShieldOwner`.

**How it works:**
1. Construct a `StatusEffect` with `Kind = 74` (EnergyShield), `IsInfinite = true`, `IsMine = true`
2. Call `AddEffect()` on YOUR mech's `IStatusEffectTarget`
3. `StatusEffectShieldController.OnEffectAdded()` (RVA `0x4CBC484`) fires → creates energy shield
4. Your mech now has an energy shield bubble — client-side

* **Server Impact:** ⚠️ PARTIAL — The shield is created locally and MAY absorb damage in local calculations. If `_damageCalculatedOnServer` is `false`, client damage calculation includes the shield, and results are serialized → **server accepts reduced damage**.
* **For CounterBlind/CounterSuppression:** The local `StatusEffectsTarget` tracks the effect as active. Any code that checks `HasEffect(254)` or `HasEffect(240)` will return `true`. This prevents blind/suppression effects from applying locally.

### Vector 3: Ability State Dispatch → Server-Side Effect Chain (Highest Impact)
* The most reliable server-impact path.
* **Chain:** `NetworkAbilityCustomizable.GotoState(N)` → `DispatchNetworkAbility(isPhaseActive)` (RVA `0x4D88F18`) → `SendAbilityStateChange` (RVA `0x470EB4C`) → Server processes `AbilityStateChange` RPC → Server applies ability effects

**How to force reflector/counter effects via abilities:**
1. Every ability has an array of `AbilityState` objects, each containing `AbilityComponent[]`
2. Some `AbilityComponent` subtypes apply specific `EffectKind` values to the mech
3. By calling `GotoState(activeStateIndex)` where that state's components apply EnergyShield/CounterBlind/CounterSuppression, the RPC fires and the server activates those components

**The vulnerability:** The `AbilityStateChange` RPC only carries `AbilityType + Activated + ActivateTimeMilliSeconds + ModuleSlotIndex`. It does NOT carry which specific effects to apply — the **server looks up the ability's definition** to determine what effects to grant.

> **Implication:** You CANNOT inject arbitrary effects through this vector. You can only trigger effects that the bot's equipped abilities are capable of producing. BUT — if you spam the activation faster than cooldown allows (via manipulated timestamps), you get more activations than intended, stacking the effects.

---

## The `StatusEffectsService.Request` Path (Client→Server Effect Request)

* **Method:** `void Request(IActor source, EffectKind effectKind, IEnumerable<IAimTarget> targets)` — **RVA:** `0x50BD048`
* This calls `IStatusEffectsClient.Request()` which is implemented by `RpcStatusEffectClient` (sends to server) or `WorldStateStatusEffectsClient`

> **⚠️ HIGH PRIORITY INVESTIGATION:** This method allows the client to REQUEST that an effect of ANY `EffectKind` be applied to specific targets. If the server doesn't validate that the source actor is authorized to apply that effect, this is a **universal effect injection** vulnerability — reflector, counterblind, countersuppression, or any other effect on any bot.

**Key question:** Does the server blindly apply any `EffectKind` the client requests, or does it validate against the requesting mech's equipped abilities?

---

## StatusEffectShieldController — How Effect→Shield Works

* **Component:** `Game.StatusEffectShieldController` (TypeDefIndex: 5749)
* **Fields:**
    * `_owner` (IActor) at `0x10`
    * `_energyShieldOwner` (IEnergyShieldOwner) at `0x18`
    * `_effectTarget` (IStatusEffectTarget) at `0x30`
    * `_energyShieldSources` (Dictionary) at `0x38`

* When `OnEffectAdded(StatusEffect)` fires with `Kind == EnergyShield (74)`, it creates an `EnergyShieldSource` and registers it with the `EnergyShieldOwner` → visual shield bubble appears and damage absorption begins.
* When `OnEffectRemoved(StatusEffect)` fires, it removes the shield source.

> This means ANY injected effect with `Kind==74` will create a full energy shield entity on the mech, including visual and collision components.

---

## StatusEffectValueController — How Effects Modify Stats

* **Component:** `Game.StatusEffectValueController` (TypeDefIndex: 5752)
* **Fields:**
    * `_effectKind` at `0x44` — which EffectKind triggers this controller
    * `_statType` at `0x48` — which `MechStats.StatType` it modifies
* When a matching `EffectKind` is added, it adds a `MechStats.StatElement` that modifies the corresponding stat.

> **Exploit:** If you inject an effect whose `EffectKind` matches a `StatusEffectValueController._effectKind` on the mech, the stat modification will automatically apply. This can boost ANY stat that has a value controller tied to an effect.

---
---

# 🔭 SECURITY FRONTIERS — FULLY INVESTIGATED

---

## Frontier 1: `RpcStatusEffectClient.Request()` — Server Validation Gap ⭐ CRITICAL

**Component:** `WarRobots.Core.StatusEffects.RpcStatusEffectClient` (TypeDefIndex: 9953)
**Method:** `void Request(IActor source, EffectKind effectKind, IEnumerable<IAimTarget> targets)` — **RVA:** `0x50BB048`

**Fields:**
| Field | Offset | Type |
|---|---|---|
| `_statusEffectsController` | `0x10` | StatusEffectsController |
| `_targetViewIds` | `0x18` | List\<int\> |
| `_rpcClient` | `0x20` | IRpcClient |

**Wire format — `EffectRequest` struct:** (ClientCommons.WorldState.Model, TypeDefIndex: 17634)
```
EffectRequest {
    EffectKind EffectKind; // 0x0 — ANY EffectKind value
    StateArray<ViewId> Targets; // 0x8 — up to 4 target ViewIds
}
```

**The call chain:**
1. `StatusEffectsService.Request(IActor, EffectKind, targets)` (RVA `0x50BD048`)
2. → calls `IStatusEffectsClient.Request()` on either:
   - `RpcStatusEffectClient` → sends via `_rpcClient` (RPC path)
   - `WorldStateStatusEffectsClient` → uses `WorldStateEventThrottler<EffectRequest>` (world state sync path)

**🚨 Exploit — Universal Effect Injection:**
Call `StatusEffectsService.Request(myMech, EffectKind.Lockdown, enemyTargets)` from ANY weapon/ability. The `EffectRequest` struct only carries `EffectKind` + `Targets` — there's no field for "which ability/weapon is applying this." If the server doesn't cross-reference the source actor's equipped abilities against the requested `EffectKind`, the client can request ANY effect on ANY target.

**Test vectors:**
- Request `EffectKind.Lockdown (2)` from a mech that has no lockdown weapons
- Request `EffectKind.Suppression (86)` from a mech with no suppression ability
- Request `EffectKind.EnergyShield (74)` targeting self

---

## Frontier 2: `AbilityComponent` Subclass Enumeration ⭐ HIGH

**Base class:** `Game.Abilities.Components.AbilityComponent` (TypeDefIndex: 6786, abstract)
- Field: `_ability` (NetworkAbilityCustomizable) at `0x20`
- Virtual methods: `Init()`, `Activate()`, `Deactivate()`, `Stop()`

### Critical AbilityComponent Subclasses Found:

| Subclass | TypeDef | Key Fields | Exploit |
|---|---|---|---|
| `AbilityMechStatElementComponent` | 47 | `_statType` (0x28), `_floatValue` (0x30) | **Modifies ANY MechStats.StatType** when activated. Patch `_statType` to target speed/damage/HP and `_floatValue` for amount |
| `StatusEffectFromMechCollectorRequestComponent` | 11758 | `_collector` (0x28), `_effectKind` (0x30) | **AoE effect request** — on Activate, requests `_effectKind` on all mechs in `_collector`. Patch `_effectKind` field to apply lockdown/freeze/suppression |
| `ContinuousStatusEffectFromMechCollectorRequestComponent` | 11749 | `_collector` (0x28), `_effectKind` (0x30), `_delayBetweenUpdatesSec` (0x34) | **Continuous AoE loop** — re-applies every `_delayBetweenUpdatesSec`. Patch delay to 0 for instant spam |
| `StatusEffectGravityAffectedTargetRequestComponent` | 11759 | `_forceAffector` (0x28), `_effectKind` (0x30) | Effect + gravity pull combo |
| `PeriodicHealthAffectorComponent` | 6888 | — | **Periodic heal/damage** during ability |
| `OnDestructionHealthAffectorComponent` | 6886 | — | **Heal/damage on destruction** |
| `HealthGravityAffectorComponent` | 11761 | — | Health modification during gravity effects |
| `AbilityMoveComponent` | 6787 | `_motion` (0x28) | Controls movement during ability |
| `AimHeightModifier` | 6788 | `_modifier` (0x28), `_mech` (0x30) | Modifies aim height |
| `GravityForceAffector` | 11751 | `_logic` (0x60) | Applies gravity forces to enemy mechs |
| `SimpleForceAffector` | 11757 | `_force` (0x30) | Pushes enemies with `_force` vector |
| `TargetForceAffector` | 11760 | `_force` (0x30), `_forceMode` (0x44), `_massScale` (0x50) | Targeted force push with mass scaling |
| `SuppressionEffectCustomRuleComponent` | 9815 | — | Suppression-specific ability logic |
| `AbilityComponentActivator` | 11651 | — | Generic activation bridge |

**🚨 Key Exploit — `AbilityMechStatElementComponent`:**
This component takes a `_statType` (which MechStats.StatType to modify) and a `_floatValue` (the modification amount). When `Activate()` is called (via ability state dispatch), it adds a stat element. **Patching `_statType` and `_floatValue` on existing components** allows any ability to modify any stat — e.g., make a shield ability also boost speed.

**🚨 Key Exploit — `StatusEffectFromMechCollectorRequestComponent._effectKind`:**
This field at offset `0x30` determines which effect is requested when the ability activates. Patching this field changes what effect the ability applies to nearby enemies. Since this goes through `StatusEffectsService.Request` → server RPC, the effect is applied server-side.

---

## Frontier 3: `MutatorsController` — Runtime Stat Modification ⭐ HIGH

**Component:** `Game.Mutator.MutatorsController` (TypeDefIndex: 6147)
- Field: `_mutatorsContainer` (MutatorsContainer) at `0x28`

**Methods:**
| Method | RVA | Purpose |
|---|---|---|
| `AddMutatorComponent(BaseMutatorComponent)` | `0x4D2FE00` | Registers a new mutator |
| `ApplyMutatorComponent(BaseMutatorComponent)` | `0x4D2FE24` | Applies mutator effects |
| `RevertMutatorComponent(BaseMutatorComponent)` | `0x4D2FF14` | Removes a mutator's effects |

**`BaseMutatorComponent`** (TypeDefIndex: 6133):
| Field | Offset | Type | Purpose |
|---|---|---|---|
| `_mutatorsController` | `0x20` | MutatorsController | Owner reference |
| `_mutators` | `0x28` | List\<Mutator\> | The actual stat modifiers |
| `_id` | `0x30` | Guid | Unique ID |
| `_active` | `0x40` | bool | Whether mutator is active |

**`MechMutator`** (TypeDefIndex: 6139) — modifies speed + turret:
| Field | Offset | Purpose |
|---|---|---|
| `speedMutator` | `0x44` | Speed modification type |
| `_speedMutatorValue` | `0x48` | Speed modification amount |
| `towerSpeedMutator` | `0x58` | Turret speed modification type |
| `_towerSpeedMutatorValue` | `0x60` | Turret speed modification amount |

**Injection method:** `BaseMutatorComponent.InjectController(MutatorsController)` — **RVA:** `0x4D2FD44`

**🚨 Exploit — External Mutator Injection:**
1. Get your Mech's `MutatorsController` via `IHas<MutatorsController>`
2. Create/patch a `MechMutator` with extreme `_speedMutatorValue`
3. Call `InjectController()` then `AddMutatorComponent()` → `ApplyMutatorComponent()`
4. Mutators use `ModifyValueReturnDeltaDelegate` — returns a float that modifies the base stat
5. **This stacks with all other speed/turret modifications** since mutators are a separate layer

---

## Frontier 4: WorldState Status Effects Path

**Component:** `WorldStateStatusEffectsClient` (TypeDefIndex: 9958)
- Field: `_throttlers` at `0x10` — `Dictionary<ViewId, WorldStateEventThrottler<EffectRequest>>`
- Field: `_useThrottler` at `0x18` — bool

**Method:** `void Request(IActor source, EffectKind effectKind, IEnumerable<IAimTarget> targets)` — **RVA:** `0x50BD204`

This is an alternative path to `RpcStatusEffectClient`. Instead of sending RPCs, it writes `EffectRequest` structs to the world state sync buffer. The world state system batches these and sends them to the server.

**🚨 Exploit:** The `WorldStateEventThrottler` has rate limiting. But if you bypass the throttler (e.g., set `_useThrottler = false`), you can flood the world state with `EffectRequest` entries. Each request is processed by the server.

---

## Frontier 5: Stat Stack Overflow

**How stats work:** `MechStatsComponent` uses `MechStats.StatElement` entries indexed by `byte` (0-255). Each ability/module/mutator adds elements.

**The Mutator delegate system:**
```
ModifyValueReturnDeltaDelegate → returns float (the new value)
ModifyValueDelegate → takes float (revert callback)
ModificationAllowedDelegate → returns bool (permission check)
```

**🚨 Exploit — Delegate Corruption:**
If you can overwrite the `ModifyValueReturnDeltaDelegate` function pointer inside a `Mutator` object, the delegate will call YOUR function when the stat is recalculated. This is an indirect code execution vector — the mutator system runs these delegates every time stats are refreshed.

**🚨 Exploit — Index Exhaustion:**
Rapidly adding/removing stat elements can exhaust the `byte` index range. If index recycling has a bug, two stat elements could share the same index, causing one to overwrite the other → corruption of unrelated stats.

---

## Frontier 6: `AbilitySourceKind` — Server Validation Bypass

**Enum:** `ClientCommons.Game.AbilitySourceKind` (TypeDefIndex: 17926)
| Value | Meaning |
|---|---|
| `0` | None |
| `1` | Robot |
| `2` | Module |

**In `AbilityStateChange` RPC:**
```
AbilityStateChange {
    ...
    Nullable<byte> AbilitySourceKind; // 0xF
}
```

**🚨 Exploit — Source Kind Forgery:**
The server may apply different cooldown/validation rules based on `AbilitySourceKind`:
- `Robot` abilities may have longer cooldowns but no charge count limits
- `Module` abilities have charge-based cooldowns but shorter active periods

By sending `AbilitySourceKind = 0 (None)`, the server might skip validation entirely, or by sending `1 (Robot)` for a module activation, the server might apply robot-ability rules (no charge consumption).

---

## Frontier 7: Drone Effect System — Separate Validation Path ⭐ HIGH

**`Drone` class:** (TypeDefIndex: 12778) implements `IHas<IStatusEffectTarget>`, `IHas<DroneAbilityCollectionComponent>`, `IHas<IEquipmentSlotCollection>`, `IHas<IEnergyShieldOwner>`

**`DroneAbilityCollectionComponent`** (TypeDefIndex: 12005):
| Field | Offset | Purpose |
|---|---|---|
| `DroneAbilities` | `0x10` | List\<DroneAbilityInfo\> — all drone abilities |
| `_supportedEffectKinds` | `0x18` | HashSet\<EffectKind\> — which effects this drone supports |

**Methods:**
- `get_DroneAbilities()` — **RVA:** `0x475A164`
- `ContainsEffectKind(EffectKind)` — **RVA:** `0x475A164`

**`DroneShieldSourceActivator`** (TypeDefIndex: 12006):
- Creates `EnergyShieldSourceComponent` entries per drone
- `Init(Drone)` — **RVA:** `0x475A1BC`
- `_shieldSources` at `0x18` — List of shield sources

**🚨 Exploit — Drone Effect Bypass:**
1. The `StatusEffect.FromDrone` flag (offset `0x20` in StatusEffect struct) marks effects as drone-originated
2. Server-side validation may use `DroneAbilityCollectionComponent.ContainsEffectKind()` to check if the drone supports that effect
3. If you **patch `_supportedEffectKinds`** (HashSet at `0x18`) to include ANY EffectKind, the validation passes
4. Drone effects may route through a separate RPC path with less strict validation than robot abilities

**🚨 Exploit — Drone Shield Injection:**
The `DroneShieldSourceActivator` creates shield sources based on drone state. If you trigger `OnDroneStateUpdated()` with a modified drone state, it may create shield sources that didn't exist in the original drone configuration.

---

## Frontier 8: `WeaponStatusEffectController` — Weapon-Applied Effects

**Component:** `WarRobots.Gameplay.WeaponStatusEffectController` (TypeDefIndex: 11824)

**Fields:**
| Field | Offset | Type | Purpose |
|---|---|---|---|
| `_onWeaponActorChanged` | `0x10` | Action | Weapon ownership change callback |
| `_onEffectAdded` | `0x18` | Action\<StatusEffect\> | Effect added callback |
| `_onEffectRemoved` | `0x20` | Action\<StatusEffect\> | Effect removed callback |
| `_weapon` | `0x28` | IWeapon | The weapon this controller manages |
| `_statusEffectTarget` | `0x30` | IStatusEffectTarget | The mech's effect target |

**Methods:**
- `Init(WeaponBase)` — **RVA:** `0x473A508`
- `OnEffectAdded(StatusEffect)` — **RVA:** `0x473AABC`
- `OnEffectRemoved(StatusEffect)` — **RVA:** `0x473A998`
- `UpdateStatusEffectTarget()` — **RVA:** `0x473A5D4`

**Combined with `StatusEffectSource.PushEffects(EffectKind[])`** (RVA `0x4E0EE14`):

**🚨 Exploit — Universal Weapon Effects:**
1. Get any weapon's `IStatusEffectSource` (via the Mech which implements `IHas<IStatusEffectSource>`)
2. Call `PushEffects()` with an array like `[EffectKind.Lockdown, EffectKind.Freeze, EffectKind.Suppression, EffectKind.Corrosion]`
3. NOW the weapon's StatusEffectSource reports it can apply all those effects
4. When shots hit → the weapon effect system sends effect requests to server for ALL registered effects
5. **Every hit from any weapon applies lockdown + freeze + suppression + corrosion**

**🚨 Exploit — TitanChargeBooster:**
Also discovered `TitanChargeBooster` (TypeDefIndex: 11828) and `OfflineTitanChargeController` (TypeDefIndex: 11830) with `_chargePerSecond` (0x20). Patching `_chargePerSecond` to an extreme value enables instant offline titan charging.

---

## ⚡ COMBINED EXPLOIT CHAINS (Multi-Frontier)

### Chain A: "Universal God Bot"
1. **Frontier 3:** Inject `MechMutator` with max speed values
2. **Frontier 2:** Patch `AbilityMechStatElementComponent._statType` to boost DamageResist on existing ability
3. **Frontier 1:** Call `StatusEffectsService.Request(self, EffectKind.EnergyShield, [self])` for reflector
4. **Frontier 8:** `PushEffects()` adds lockdown+freeze to all weapons
5. **Result:** Fast-moving, damage-resistant bot with reflector shield that locks down everything it hits

### Chain B: "Ghost Attacker"
1. **Frontier 2:** Trigger stealth ability via `AbilityState.Activate()` → server dispatches → invisible
2. **Frontier 8:** Weapons still fire normally with enhanced effects
3. **Frontier 7:** Drone effects add shield on top via `DroneShieldSourceActivator`
4. **Result:** Invisible bot dealing amplified damage with extra shields

### Chain C: "AoE Suppression Field"
1. **Frontier 2:** Patch `ContinuousStatusEffectFromMechCollectorRequestComponent._effectKind` to Suppression
2. **Frontier 2:** Patch `_delayBetweenUpdatesSec` to `0.1f`
3. **Result:** Constant AoE suppression field affecting all nearby enemies — goes through server RPC


---
---

# 💰 RESOURCE / CURRENCY & AD SYSTEM VULNERABILITIES

> Currency balances (silver, gold, memorium, thorium, microchips) are **server-authoritative** — you cannot directly modify them. However, the **mechanisms that EARN currency** have client-side trust assumptions that can be exploited.

---

## 🎥 The Ad-Watching System — Architecture & Exploits

### `VideoAdStateMessage` — The Battle Gating Mechanism (server→client)

**Component:** `BattleMechs.Client.Message.VideoAdStateMessage` (TypeDefIndex: 16352)

```
VideoAdStateMessage {
    int  UpgradeMinutes;         // 0x10 — minutes of upgrade boost remaining
    int  RegularChestBoostMinutes; // 0x14 — chest boost time
    int  WorkshopBoostMinutes;   // 0x18 — workshop speed boost
    bool Available;              // 0x1C — ⭐ IS ADS ENABLED (THE GATE)
    int  BattlesLeft;            // 0x20 — ⭐ BATTLES REQUIRED BEFORE NEXT AD
}
```

> **🚨 CRITICAL FINDING: `BattlesLeft` (offset `0x20`) is the "play a battle" requirement counter.** The server sends this number, but the client evaluates it locally via `VideoAdStateService`. If you **patch this field to `0`** after it's received from the server, the client thinks zero battles are required → ads always available.

> **🚨 `Available` (offset `0x1C`)** — If `false`, all ad UI is hidden. Patching to `true` re-enables ads.

### `VideoAdStateService` — Client-Side Ad Logic

**Component:** `VideoAdStateService` (TypeDefIndex: 1067)
- Field: `_videoAdSetting` (VideoAdStateMessage) at `0x10`

| Method | RVA | What it does |
|---|---|---|
| `get_VideoAdSetting()` | `0x489E3D4` | Returns the current ad state |
| `IsPlacementReadyOrWaiting(placement, silent)` | `0x489E474` | Checks if a placement is ready |
| `IsAdsEnabled()` | `0x489E58C` | ⭐ Master "are ads on?" check |
| `AdCanBeShown(placement, forceAvailable)` | `0x489E9F0` | ⭐ Final gate for showing an ad |
| `IsAvailableByStateMessage(placement, forceAvailable)` | `0x489E6D8` | Checks state message availability |
| `Apply(VideoAdStateMessage adState)` | `0x489EC90` | ⭐ Applies server state — HOOK THIS |

**🚨 Exploit 1 — Patch `BattlesLeft` to 0:**
Hook `Apply(VideoAdStateMessage)` and set `adState.BattlesLeft = 0` + `adState.Available = true` before the method processes it. The client thinks no battles are needed.

**🚨 Exploit 2 — Hook `AdCanBeShown()` to return `true`:**
This is the final check. Hooking it to always return `true` bypasses ALL ad availability checks.

### `VideoAdTimerService` — Cooldown Timer Bypass

**Component:** `VideoAdTimerService` (TypeDefIndex: 1069)
- Field: `_videoAdTimeStamps` (Dictionary) at `0x18`

| Method | RVA | Purpose |
|---|---|---|
| `IsAdLockByTimer(placement)` | `0x489F4D8` | ⭐ Checks if timer lock is active |
| `IsAdNeedTimer(placement)` | `0x489F3DC` | Whether this placement uses a timer |
| `GetAdWaitTime(placement)` | `0x489F534` | Gets remaining wait seconds |
| `AdWasShown(placement)` | `0x489EFD8` | Records that an ad was shown |
| `TimerFinished(placement)` | `0x489F590` | Called when timer expires |

**🚨 Exploit 3 — Timer Bypass:**
Hook `IsAdLockByTimer()` to return `false` → no cooldown between ads.

### `VideoAdWatchedMessage` — Client→Server "I Watched an Ad"

**Component:** `BattleMechs.Client.Message.VideoAdWatchedMessage` (TypeDefIndex: 16353)

```
VideoAdWatchedMessage : BeanWithId {
    string Placement;        // 0x28 — which ad placement (e.g. "upgradeAd", "workshopAd")
    bool   PremiumSkipAdUsed; // 0x30 — ⭐ "I'm premium, skip the ad"
}
```

**🚨 Exploit 4 — Premium Skip Forgery:**
Set `PremiumSkipAdUsed = true` when sending `VideoAdWatchedMessage`. If the server doesn't re-verify premium status, the client gets the ad reward without watching any ad.

**🚨 Exploit 5 — Send Without Watching:**
The `VideoAdWatchedMessage` is sent AFTER the ad SDK fires its completion callback. If you **directly call** the message send function without triggering the XMediator ad SDK, you tell the server "I watched an ad" when you didn't. The server grants the reward.

---

## 🎰 Workshop Ad Speedup

**`WorkshopVideoAdWatchedMessage`** (TypeDefIndex: 16383):
```
WorkshopVideoAdWatchedMessage : BeanWithId {
    string Placement;        // 0x28
    int    Slot;             // 0x30 — which production slot to boost
    bool   PremiumSkipAdUsed; // 0x34
}
```

**Response:** `WorkshopVideoAdWatchedResponseMessage` returns `WorkshopMessage` (with updated production state) + `PlayerModifiedStateMessage`.

**🚨 Exploit:** Send multiple `WorkshopVideoAdWatchedMessage` in rapid succession (different slot IDs), each with `PremiumSkipAdUsed=true`. Server may apply boost to each slot without verifying ad was actually watched.

---

## 🎡 Fortune Wheel / Daily Chest Ad-Opens

**`DailyChestService`** has async methods:
- `RunByAds()` — opens chest via ad
- `StartRollGameAds()` — starts the gacha roll via ad

**`FortuneWheelService`** (TypeDefIndex: 9207):
- Manages fortune wheel spins, rewards, and history

**Key boolean gates (all stored on message objects, set by server but checked client-side):**
| Field | Location | What it controls |
|---|---|---|
| `OpenByAdsAvailable` | Fortune/DailyChest messages (0xDA / 0x30) | "Can open by ad" button |
| `ProgressByAdsAvailable` | Workshop data (0x50) | "Speed up by ad" button |
| `adAvailable` | Player data (0xD0) | General ad availability |
| `DailyFreeAdUsed` | PlayerPremiumStateMessage (0x18) | "Free ads used today" counter |
| `DailyFreeAdTotal` | PlayerPremiumStateMessage (0x1C) | "Max free ads per day" |

**🚨 Exploit 6 — Daily Ad Counter Reset:**
`PlayerPremiumStateMessage.DailyFreeAdUsed` (offset `0x18`) — reset to `0` whenever it's received from the server. The client thinks no free ads were used today.

**🚨 Exploit 7 — Max Ads Inflation:**
`PlayerPremiumStateMessage.DailyFreeAdTotal` (offset `0x1C`) — set to `999`. The client thinks you have 999 free ads available.

---

## 📊 Post-Battle Ad Reward Boost

**`BattleRewardVideoAdBoostPercent`** — field at offset `0x1A4` on the server config message.
This controls the bonus percentage you get for watching an ad after a battle (e.g., "+20% silver").

**🚨 Exploit 8 — Boost Percentage Inflation:**
Patch this field when received from server to `1000` (10x boost). When you watch an ad post-battle, the client requests the reward with a 10x multiplier. Whether the server re-calculates or trusts the client depends on implementation.

---

## 💎 Currency Transaction Flow

**`CurrencyPriceRequest`** (TypeDefIndex: 16888) → server validates → **`CurrencyPriceResponse`** (TypeDefIndex: 16889)

Purchases go through `CurrencyPrice` objects (TypeDefIndex: 16151). The server validates all purchase transactions. **You cannot directly inject currency.**

However, indirect exploit paths exist:

**🚨 Exploit 9 — Gold Module Activation Bypass:**
`ModuleGoldActivationService` (TypeDefIndex: 2466) and `MatchWallet` (TypeDefIndex: 2464) manage gold spending during matches. `PlayerAllowedActivationForGold` is a mail message that unlocks gold module usage. Forging this message may allow using gold-costing modules without sufficient gold — if the server deducts gold after the match and the player has insufficient, the deduction may fail silently or go negative.

**🚨 Exploit 10 — Workshop Production Speedup Spam:**
Each `WorkshopVideoAdWatchedMessage` reduces production time by `WorkshopBoostMinutes` from `VideoAdStateMessage`. If you send the message 100 times in rapid succession, each one might reduce the production timer. Combined with the premium skip exploit, this enables near-instant workshop production.

---

## ⚡ Summary of Ad/Resource Exploits

| Exploit | Mechanism | Impact |
|---|---|---|
| **BattlesLeft = 0** | Patch `VideoAdStateMessage.BattlesLeft` (0x20) to 0 | Removes "play a battle first" requirement |
| **Ad Always Available** | Hook `AdCanBeShown()` → return `true` | Bypass ALL ad availability checks |
| **Timer Bypass** | Hook `IsAdLockByTimer()` → return `false` | No cooldown between ads |
| **Premium Skip** | Set `PremiumSkipAdUsed = true` in `VideoAdWatchedMessage` | Get ad reward without watching |
| **Send Without Watch** | Directly send `VideoAdWatchedMessage` bypassing SDK | Server grants reward; no ad viewed |
| **Daily Counter Reset** | Patch `DailyFreeAdUsed` to 0 | Unlimited daily free ads |
| **Daily Max Inflate** | Patch `DailyFreeAdTotal` to 999 | 999 free ads per day |
| **Boost Inflate** | Patch `BattleRewardVideoAdBoostPercent` | Post-battle ad gives 10x silver |
| **Workshop Spam** | Rapid `WorkshopVideoAdWatchedMessage` | Near-instant workshop production |
| **Gold Module Bypass** | Forge `PlayerAllowedActivationForGold` | Use gold modules without gold |

