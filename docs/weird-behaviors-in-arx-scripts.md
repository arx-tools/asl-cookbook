# Weird behaviors in Arx scripts

## [bug] ^rnd_0 will hang Arx in versions before Arx Libertatis 1.3

Before Arx Libertatis 1.3 calling `^rnd_0` will cause the game to go in an infinite loop and hang requiring a force
quit. In later versions it returns `0.000000` as expected.

## [bug] Conditions in one line will trigger an error

Calling a condition in one line, like:

`random 50 { speak -p [goblin_dying] }`

will trigger the following error and skip executing subsequent code in the given block:

`[W] ScriptEvent:368      [player:36] executeline: <-- event block ended without accept or refuse!`

Putting the code within the if/else blocks to a separate line solves the issue:

```
random 50 {
  speak -p [goblin_dying]
}
```

The same can be applied when also using an else clause:

```
if (^rnd_100 > 50) {
  speak -p [goblin_dying]
} else {
  speak -p [goblin_hail]
}
```

## [bug] Running timers in Arx Libertatis console will trigger an infinitely repeating error

Running the following code - or any other timed commands:

`timerasdf -m 1000 speak -p [goblin_hail]`

in the Arx Libertatis console will cause the following message to be repeated infinitely:

`[W] ScriptEvent:379      [player:26] executeline: <-- unknown command: �`

## [inconsistency] play and speak behave differently when having to execute a command after audio playback

**Note that this is documented in the wiki**

Writing a command after speak will make the command wait for the end of the audio playback.

```
speak [goblin_misc] herosay "hello"
```

Doing the same with audio from the sfx folder will not work, the command will run immediately:

```
play "ylside_death.wav" herosay "hello"
```

One tedious workaround is to use a timer after the play command with a time that kinda matches the length of the audio:

```
// sfx/ylside_death.wav plays for around 9 seconds
play "ylside_death.wav" TIMERdelay -m 1 9000 herosay "hello"
```

Of course this breaks down when the play command uses the `-p` flag which randomizes the pitch/speed between 90% and 110%

Read more at: https://wiki.arx-libertatis.org/Script:play#Audio_playback_command_comparison

## [bug] showlocals / showglobals / showvars has an issue displaying variable types in Arx Libertatis console

Some characters - ironically the most important ones which help determine the type of the variable - don't show:

```
[C] Console:405           > marker_0001.showlocals
[I] ScriptedInterface:215  Local variables for marker_0001:
�hud_line_1 = "player size: 50.00cm"
�hud_line_2 = " "
�hud_line_3 = " "
�hud_line_4 = " "
@tmp = 0
�wholepart = 50
�decimaldigit1 = 0
�decimaldigit2 = 0
�tmp = 0
```

Specifically the ones which are unique to the ISO-8859-15 character encoding: `§` (int) and `£` (string)

## [inconsistency] spellcast command

- The `-x` flag mutes the sound of only 4 spells: Heal, Detect Trap, Armor and Lower Armor. There's a
  [pull request](https://github.com/arx/ArxLibertatis/pull/293) which aims to implement it for all other
  spells aswell.
- The `-m` flag is supposed to hide the rune drawing part of spells, but it does not work everywhere,
  for example the Curse spell is always drawn.
- The radius of how far a spell's effect reach is determined by the player's casting skill. See this demonstrated
  using Ignite and Douse in this video: https://www.youtube.com/watch?v=y63RQ5FAy40

Read more at: https://wiki.arx-libertatis.org/Script:spellcast

## [bug] curse spell

The texture of the woodoo doll (`graph/obj3d/textures/(FX)_POUPéE.jpg`) cannot be replaced, probably because of the
`é` in its filename. It seems to work fine

## [limitation] Not all spells emit particles when too many spells are cast parallel

There seems to be a limit to how many spells can emit particles at the same time.

![Not all spells emit particles](img/not-all-spells-emit-particles.jpg?raw "Not all spells emit particles")

## [limitation] halo's light effect only shows up around a limited number of entities

Not every entity can have halos around them, most of them will remain invisible.

Every star on the image below is supposed to have a large halo around it:

![Not all stars have halos](img/number-of-halos.jpg?raw "Not all stars have halos")

The same limitation can be observed multiple times in the following video, where respawning guards will get a halo
around them briefly: https://www.youtube.com/watch?v=w_Lnp1C2Pd8

**Note that halo still makes an entity glow, only the flare effect is not rendered correctly**

## [undocumented] behavior none doesn't seem to behave as expected

Let's say you want to make an NPC face towards the player. You can do that with the following 2 commands:

```
behavior friendly
settarget player
```

Afterwards you should be able to turn the friendly behavior off to stop an NPC from looking at the player. However
this does not work:

```
behavior none
```

Although one would expect this to be the solution, as `behavior none` can be seen 200 times in the original scripts.

Instead the workaround is the following:

```
// making the NPC face the player:
behavior stack
behavior friendly
settarget player

// revert the behavior back:
behavior unstack
```

Currently neither `behavior` or `settarget` is documented in the Arx Libertatis wiki, so we can only rely on what's
inside existing scripts.

## ^dist\_<entity> is unreliable

I have `marker_0004` placed at `60000/0/10000` and `marker_0005` placed at `10000/0/10000`.
From within `marker_0005` I call `herosay ^dist_marker_0004` and instead of `4000` I get `4001.125`
