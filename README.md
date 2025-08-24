## Lily - a grimoire of fighting game recordings

These recordings are intended to be played back using [Eddienput](https://github.com/nirgoren/Eddienput). You should use v2.0-beta of Eddienput, which you can [download here](https://github.com/nirgoren/Eddienput/releases/tag/v2.0-beta). The recordings are most useful for:

- Training defense against specific setups.
- Verifying if old combos still work & generate the same resources across patches.
- Using as examples for improving your own execution (e.g., these recordings will typically be of the "most ambiguous" timing of most manually timed mixes, allowing you to compare against the recordings).
- Documenting setups that are very subtly different and may not be as easy to document in text.
- Quickly recording tech videos of a diverse range of setups.
- Verifying if small changes in a route have changes in outcomes.

## The first-time Eddienput user guide

- Download v2.0-beta of Eddienput [from here](https://github.com/nirgoren/Eddienput/releases/tag/v2.0-beta).
- Unzip it somewhere you can find it later.
- Run the installer named `ViGEmBusSetup_x64`. This is the controller emulator - without it installed, Eddienput can record your physical controller but not playback recordings through the virtual controller.
- Restart your computer - the virtual controller driver does require a restart.
- Run the `Eddienput` executable _before you start Strive_. Make sure there's no red error text in the log. It should say something like:

```
XInput controller detected! Recording is enabled
Loaded playbacks from playbacks\recording.txt
Eddie is ready (1)
```

- Start Strive and enter training mode.
- Set the training dummy to use controller input (pause > training settings > opponent status > opponent state).
- Press `Insert` on your keyboard to enter Eddienput's "manual" mode. While in manual mode, some keys on the keyboard are mapped to controller inputs on Eddienput's virtual controller. You can see the key mappings at the bottom of Eddienput - while in manual mode, there's a graphic of the keybindings. Manual mode has huge input delay and drops most inputs - it's not usable for playing, but it can be used for small menu interactions.
- Press `Home` - this is bound to Start - and navigate using the arrow keys and `X` (which is bound to a controller's `A` button) to get into the keybinding menu. From there, set the keybindings correctly - I recommend setting them to the keybindings you use during normal play.
- Exit the menus and press `Insert` to exit Eddienput's manual mode.
- Press `F10` to start recording your physical controller. Do whatever setup you want recorded, then press `Select` on your physical controller to end the recording. The recording is saved to `...Eddienput/playbacks/recording.txt`. If you want to keep it, make sure to copy and rename it - otherwise, your next recording will overwrite it.
- Reset training mode and press `F3` to replay the recording. Congrats - you now know enough to be dangerous with Eddienput!

## Troubleshooting Eddienput

- None of my changes to `recording.txt` do anything!
  - Check the logs; your recording file probably has some incorrect syntax in it. In this case, Eddienput will continue playing back the most recent correct recording.
- Strive no longer recognizes the virtual controller.
  - Put the virtual controller in manual mode (`Insert`) and mash start (`Home`) a few times to get to the menu. If you can get to the menu, playback will work again - if not, restart Eddienput.

## Working with Eddienput recording files

### How to think about Eddienput

Eddienput is a _recorder_ for your physical controller, and a _virtual_ controller you can play those recordings back on. It records and plays back using "wall clock time," not "game logic time," so recordings will include delays based on, e.g., hitstop. When calculating (or guess-and-checking) the duration of pauses ("waits") in recordings, remember you'll have to include hitstop - the frame data on dustloop uses game logic time and will undercount.

Eddienput _does not_ connect with Strive in any way - it's just pretending to be a controller to Windows. One unfortunate quirk of this is that it can be off by 1 frame (ahead or behind) of the game. It should never be more than 1 frame off, but know that it is possible - the same recording played twice can have different outcomes. One of your goals is to make recordings that are (within reason) tolerant of this. For instance, if you have a combo where you land and then link a button after landing recovery, you'll want your recording to hold the button for several frames. If you have it press the button for one frame and you choose the first actionable frame, the combo will drop if Eddienput gets 1 frame ahead of the game logic.

### How to read an Eddienput recording file

Eddienput recording files start with a line that tells it which config to use (`configs/ggst.json`) - this tells it what button names map to what physical controller buttons, and it's why we can use standard anime notation in the recording file.

After that is a list of space or line break delimited instructions - each instruction takes at least 1 frame. For instance, `P P P` tells Eddienput to input punch every frame for 3 frames. You can input multiple things on a frame by using `+`, so `4+FD+D` inputs 4, FD macro, and dust (a wakeup block/throw OS) for one frame. You can also hold buttons for multiple frames with `[]`. `[4+FD] W10 ]4+FD[` is a 12 frame long instruction that holds FD for 11 frames - note that releasing the buttons at the end counts as a frame.

You can add comments as well - any line that starts with a `#` is a comment intended for human readers, and is ignored by Eddienput.

Eddienput will also input a beep if you insert the instruction `beep`, which you can use as a timing cue. It also supports [looping behaviors](https://github.com/nirgoren/Eddienput?tab=readme-ov-file#mixups) and [mixups (randomly chosen forks)](https://github.com/nirgoren/Eddienput?tab=readme-ov-file#mixups)

### How to "clean" a recording up

0. Start by recording yourself playing - don't worry about inputing things cleanly, just get the route right.
1. Open the recording file and add a line break after every wait `Wx`.
2. Start grouping things that belong together on the same line - 236/214 inputs, pressing and releasing buttons, etc. As you make each change, run the recording again in-game to confirm that it still works.
3. Make sure to hold buttons in the recording; resist the temptation to make them single-frame inputs. For example, `[2] W2 [K] W10 ]2+K[` is a significantly better recording than `2+K W14`. Logically they're the same, but the hold inputs are more resilient if Eddienput is 1 frame ahead or behind the game.
4. Consolidate waits in the middle of buttons for readability. For instance, `[K] W10 ]K[` is significantly better than `[K] W6 ]K[ W4` despite being equivalent.
5. Consolidate common pieces and save copies of them in `./playback/building_blocks`.

## This did not slake my lab hunger. What other tools are there?!

- [Patch selector](https://gamebanana.com/mods/553664) for checking things against historical versions of the game.
  - Installation / usage guide [here](https://docs.google.com/document/d/e/2PACX-1vTJMJrTZjA7eDyiwdnkCSeaXGoJ3d1hZabHAu56-Fnc8gIeLyvyloWST0i1uqy_Gax5dsjbHdCQUpou/pub).
  - This is pretty buggy - FRRC and mirazh shortcut inputs don't work.
- [Tension gain calculator](https://gamebanana.com/mods/609710) for comparing tension / burst gain of combos against each other.

## How can I contribute?

Honestly, I'm not there yet.

## To-do

- Document changing recording config.
- Document how to use the files in this repo (what goes where?).
- Add recording config to the repo.
- See if it's possible to build Eddienput locally to improve it (macros are very half-baked).
