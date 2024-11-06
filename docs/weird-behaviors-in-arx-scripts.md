# Weird behaviors in Arx scripts

## ^rnd_0 will hang Arx in versions before Arx Libertatis 1.3

Before Arx Libertatis 1.3 calling `^rnd_0` will cause the game to go in an infinite loop and hang requiring a force quit.
In later versions it returns `0.000000` as expected.

## Conditions in one line will trigger an error

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

## Running timers in Arx Libertatis console will trigger an infinitely repeated error

Running the following code - or any other timed commands:

`timerasdf -m 1000 speak -p [goblin_hail]`

in the Arx Libertatis console will cause the following message to be repeated infinitely:

`[W] ScriptEvent:379      [player:26] executeline: <-- unknown command: �`
