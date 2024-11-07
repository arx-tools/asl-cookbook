# Weird behaviors in Arx scripts

## [bug] ^rnd_0 will hang Arx in versions before Arx Libertatis 1.3

Before Arx Libertatis 1.3 calling `^rnd_0` will cause the game to go in an infinite loop and hang requiring a force quit.
In later versions it returns `0.000000` as expected.

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

read more at: https://wiki.arx-libertatis.org/Script:play#Audio_playback_command_comparison

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
