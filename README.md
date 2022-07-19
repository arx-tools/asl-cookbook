# ASL Cookbook

Practical examples on how to use the Arx Fatalis Scripting Language

## Enabling the console in Arx Libertatis 1.2+

https://wiki.arx-libertatis.org/Script_console

## Some useful commands

### Heal the player / give mana

+25 HP for the player

```asl
specialfx heal 25
```

+25 mana for the player

```asl
specialfx mana 25
```

### Teleport the player

immediately teleport the player to level 15, no clicking on stair icon is needed
"doesnt_matter" can be replaced by an entity id

```asl
teleport -nl 15 doesnt_matter
```

teleport player to level 18, rotate to 180deg with clicking on stair prompt
"doesnt_matter" can be replaced by an entity id

```asl
teleport -al 180 18 doesnt_matter
```

### Add gold

player gets 1000 gold

```asl
addgold 1000
```

### Open any door

Open the door to akbaa in the bunker

```
sendevent open light_door_0038 nop
```

Open the door in the tavern

```
sendevent open light_door_0073 nop
```

### Resize the player

make player 10% of it's original size - note that it doesn't change the scale of carried items

```asl
setscale 10
```

double the player's size

```asl
setscale 200
```

reset player's size back to default

```asl
setscale 100
```

### Invulnerability

makes player be immunable to damages

```asl
invulnerability -p on
```

turns off player's invulnerability

```asl
invulnerability -p off
```

turn off Iserbius'/half Akbaa's invulnerability

```asl
akbaa_phase1_0001.invulnerability off
```

### Arx equivalent of console.log

writes "hello" on top of the HUD

```
herosay "hello"
```

### Change player model

Goblin

```
usemesh goblin_base/goblin_base.teo
```

Goblin lord

```
usemesh goblin_lord/goblin_lord.teo
```

### Spawn stuff

Wall block at the player's feet

```
spawn item special/wall_block/wall_block player
```

One handed sabre at the player's feet

```
spawn item weapons/sabre/sabre player
```

### Showing hidden entities

```
objecthide akbaa_phase_0002 no
```

### Respawning

Respawn dead player (thanks @dscharrer)

```
setplayercontrols on specialfx heal ^player_maxlife
```

Respawn Lunshire, the human king

(after this the INITEND event will re-run)

```
human_base_0082.revive -i
```

### Controlling a camera

Switch to viewing the world through camera_0020

```
cameraactivate camera_0020
```

Turning off any ongoing camera

```
cameraactivate none
```

### Add items into player inventory

Adding full set of stealth armor

```
inventory playeradd armor/legging_leather_stealth/legging_leather_stealth
```

```
inventory playeradd armor/chest_leather_stealth/chest_leather_stealth
```

```
inventory playeradd armor/helmet_leather_stealth/helmet_leather_stealth
```

### Teleporting to different locations

Castle

```
teleport -l 0 marker_0338
```

City of Arx

```
teleport -l 11 marker_0222
```

Tavern

```
teleport -l 12 marker_0549
```

Teleport to the castle without prompt

```
teleport -ln 0 marker_0338
```

### Harm an entity or player

Do 10 points of damage to the player

```
dodamage player 10
```

Do minimal damage to Lunshire (when in the castle)

```
dodamage human_base_0082 1
```

### Collision

Go through the locked door in the Tavern by disabling collision

```
light_door_0073.collision off
```

## Sources

- https://wiki.arx-libertatis.org/Arx_scripting_language
- https://wiki.arx-libertatis.org/Script:Variables
- https://wiki.arx-libertatis.org/Script:Events
