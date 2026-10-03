# DELTARUNE 3DS edition (an unofficial port)


https://github.com/user-attachments/assets/2a29e960-f839-44dd-963c-8e89018e7409


This repository hosts our 3DS fan port of DELTARUNE (currently only Chapter 1 and the Menu). This project started in July 2025, and today, it is finally ready for its first public release!

It is a native port, meaning that the game's logic has been adapted and compiled to run as native 3DS machine code. This is different from the original GameMaker format, where the code is compiled as bytecode that a platform-native runner executes. The main advantage of a native port is that, since all the game logic is in native format, the code is expected to run faster since it is compiled into the console's native CPU instructions, while a runner port needs to interpret the bytecode to execute the right operations, which adds overhead and leads to slower execution.

A native port also grants more low-level control through regular game code, allowing for more fine-grained optimizations. The downsides are that the game logic has to be manually transpiled from GML to C/C++. GML is a high-level language with many useful features for game development (dynamic typing, `with` statements, magic built-in variables, etc.) that are for the most part not available in low-level languages.

On the other hand, a runner port allows the same bytecode to run on different platforms. Additionally, if a runner works for one game, the same runner can be reused to run other games.

We chose to go for a native port for several reasons:

- The game's code is sometimes... not very optimized (fine for a potato computer, not for a small console), while also relying on a "high" amount of available memory (which our beloved potato chip does not have). We can therefore make some tweaks to improve performance.

- Precise logic control.

- Implementing a 3DS game engine.

We've also implemented some cool additional features:

- Savestates (dumping the entire game context into a file, then reloading it later. Somewhat similar to an emulator savestate).

- Touch controls (overworld movement, fights, and most dialogues for now).

- Input recording functionality (recording what you press or touch so it can be replayed later).

- Second-screen minimap (WIP).

Additionally, our port is fully cross-compatible with the PC version of the game! This means you can drop your save files into `sdmc:/DELTARUNE/` to continue your adventure!

## DISCLAIMER

This project is purely a fan project, with no affiliation with Toby Fox or Nintendo.

The installation requires downloading and executing tools, and comes with no guarantees. By following the installation procedure, you acknowledge that we won't be held responsible for any damage you may cause to your computer or console. Always know what you're doing. In case of doubt, don't hesitate to ask for help.

We have done our best to provide the best experience possible. However, be aware that you may encounter bugs. They can range from basic issues like visual or audio glitches to more inconvenient problems such as framerate drops, softlocks, or crashes. We apologize for this, and we'll try to fix most of these issues in future updates.

Additionally, if you see your screen being filled with FRIEND, don't panic.
It is a recovery solution we implemented that will try to create a savestate so that you can resume your progress more easily in case of fatal exception. It is not guaranteed, but it's better than nothing. If you want to load this savestate, its id is `1225`. Refer to the [Additional](#additional) section for more information on using savestates.
<details><summary>Example video</summary>


https://github.com/user-attachments/assets/ef087906-ca13-4855-b5ad-72aec2398b77


</details>

We would have liked to share all our source code, but since, by design, a native port is heavily linked to the original game's source code, we're not sure if we're legally allowed to share it. So, in the meantime, we have found a way for you to package the `.cia` and `.3dsx` files using the provided `.xdelta` files and the DeltaPatcher tool. Please refer to the [Installation](#installation) section.

## Installation

<details><summary>If you struggle with the installation process, please have a look at these video tutorials we made for you.</summary>

<details><summary>Windows tutorial</summary>

https://github.com/user-attachments/assets/1f831353-909b-4fc4-ba90-b452704336b8

</details>

<details><summary>Linux tutorial</summary>


https://github.com/user-attachments/assets/edca811f-a680-4e86-a3b2-55034774b8b9


</details>

</details>

1) Download the latest version of our provided `.xdelta` files [here](https://github.com/LinkTroisUn/DELTARUNE-3DS/releases).

2) Download the [latest version of DeltaPatcher](https://github.com/marco-calautti/DeltaPatcher/releases).

The following steps need to be repeated for every 'game' you want to play. In case you didn't know, each DELTARUNE chapter is a separate game represented by a `data.win` file. The menu you see each time you launch DELTARUNE, which asks you to choose which chapter you want to play, is also a separate game. That's why there is a `data.win` file inside each chapter subdirectory, as well as an additional one at the root of the DELTARUNE directory.

3) Make a copy of the `data.win` of the game you want to build. The `data.win` file must come from the latest Steam version of DELTARUNE. (Currently, only Chapter 1 and the Menu are available.)

4) Use DeltaPatcher to apply the corresponding `.xdelta` patch file to the copy. You can build either a `.cia` or a `.3dsx`; choose your preferred format.

5) Rename the patched `data.win` to something like `Chapter1.cia`/`Chapter1.3dsx` (or `Menu.cia`/`Menu.3dsx`) and copy it to your console's SD card. The name is not really important, but the file extension is.

Congratulations! You are ready to play.

## Controls

### Basics

- A -> Confirm / Examine

- B -> Cancel / Run

- X -> Open menu

- D-Pad -> Movement

- Circle Pad -> Movement

### Additional

- Touch controls: In combat, touching a UI button triggers it. If it was a mistake and you want to go back, you can touch anywhere above the battle UI (or press B).

- Screen swapping: Pressing START swaps the top and bottom screens. As a safety measure, touch controls are disabled when the main screen is on top and re-enabled when the main screen is on the bottom.

Pressing SELECT + L/R triggers a special action.

The L button is associated with "loading", while the R button is associated with "recording".

The action associated with the button you release first (SELECT or L/R) will be performed.

- Savestates: If L/R is released first, the game will attempt to Load (or Record) a savestate. A keyboard prompting for a number will appear. It corresponds to a savestate slot (you can have many; they are stored in `sdmc:/DELTARUNE/savestate*`). The game will then freeze for about 10 to 30 seconds before resuming. Savestates are very tied to a specific build version. Not all updates break compatibility with older savestates, but it may happen easily. If an existing savestate gets immediately rejected, it probably means it's too old.
<details><summary>Savestates usage demo</summary>


https://github.com/user-attachments/assets/06f91194-1a6d-44e4-ac0f-0d602c8ed398


</details>

- Input save and replay: If SELECT is released first, the game will attempt to Load (or Record) a recording of the button and touchscreen inputs. When recording, press "SELECT + R" again to stop recording. When replaying, you won't be able to influence the game with your own inputs until the replay ends (or you abort it by pressing "SELECT + L"). Contrary to savestates, input recording is very stable across updates. That does not mean input recordings will always be compatible with new versions, as bug fixes, notably event-loop reordering, may break compatibility at some point. The recordings are stored in `sdmc:/DELTARUNE/3DS.recording*`.
<details><summary>Input replay usage demo</summary>


https://github.com/user-attachments/assets/1f185e8c-bb67-45fb-81e7-3859f5662f6c


</details>

## FAQ

### Q: I found a bug! Will you patch it?

**A:** Please open a [bug report ticket](https://github.com/LinkTroisUn/DELTARUNE-3DS/issues/new?template=bug_report.yml) and help us improve our port!

### Q: Why are some texts so difficult to read?

**A:** The original resolution of DELTARUNE is 640×480, while the 3DS upper screen has a resolution of 400×240 and the lower screen has a resolution of 320×240 (incredible!), which means that at some point, a downscale is required. We are not wizards, only developers, and pixels get lost in darkness during rescaling. Throughout development, we have reworked the rendering pipeline several times to reduce the quality loss of graphics, and this is so far the best rendering we have managed to achieve. That said, if you play on an emulator and double the resolution of the emulated screens, you will notice that the game looks as sharp as the PC version.

### Q: What's the difference between the .cia and .3dsx versions?

**A:** On a real 3DS, .3dsx files can only be launched through the Homebrew Launcher, whereas .cia files can be installed so that you can launch your game directly from the Home Menu. Additionally, features like “Return to Title” or “Chapter Select” will only work if you have installed both the Menu and the Chapter .cia versions.


If you're playing on an emulator, .3dsx files are more convenient since you don't have to install them, unlike .cia files. The aforementioned features won't work anyway, even if you play the .cia version.


As for savestates, if for some reason you decide to play both the .3dsx and .cia versions of a chapter, keep in mind that they are not compatible with each other. For example, if you create a savestate on the .3dsx version, you won't be able to load it properly on the .cia version.

### Q: I want that X cool feature! Can you implement it?

**A:** No. Not right now, at least. Our top 3 priorities before we consider adding new features are:

- porting Chapter 2;

- patching bugs reported by players in Chapter 1 and the Menu;

- finishing our minimap feature for Chapter 1.

You can still make a [feature request](https://github.com/LinkTroisUn/DELTARUNE-3DS/issues/new?template=feacher_request.yml), but keep in mind it will not be a priority for now.

### Q: When will you release the other chapters?

**A:** With Chapter 1 (and the Menu) released, we can now move on to porting Chapter 2. As the biggest part of porting Chapter 1 consisted of figuring out how the GameMaker engine worked and making our own engine based on it, the development of future chapters should be much quicker.

### Q: I really want to look at the source code! Can you publish it?

**A:** We do not own any rights to the DELTARUNE source code, and the code we produced for the game objects (enemies, events, …) is very close to it. Without the agreement of the DELTARUNE team, we don’t want to take any risks. We will try to contact them to ask for their permission. In the meantime, we may eventually find a way to release the code of our custom game engine separately from the other files, but it is not a high priority for us right now.

## Credits

### Special

- Toby Fox and the [DELTARUNE](https://deltarune.com/) team, owners of the wonderful game they have built.

### Tools

- [UndertaleModTool](https://github.com/UnderminersTeam/UndertaleModTool), the decompiler used to analyze the game.

- [devkitPro](https://devkitpro.org/) and the devkitARM 3DS toolchain.

- [makerom](https://github.com/3DSGuy/Project_CTR/), used for packaging the final CIA.

- [bannertool](https://github.com/diasurgical/bannertool/), used for making simple 3DS banners.

- [3dstool](https://github.com/dnasdw/3dstool), used for packaging the ROM.

- [VGAudio](https://github.com/Thealexbarney/VGAudio/), used for converting sounds to the 3DS DSP audio format.

### Dependencies

- [libctru](https://github.com/devkitPro/libctru), C 3DS user library.

- [citro3d](https://github.com/devkitPro/citro3d/), C library for using the 3DS GPU.

- [citro2d](https://github.com/devkitPro/citro2d), C library for 3DS GPU 2D graphics drawing (using custom modified sources).

- [inicpp](https://github.com/dujingning/inicpp), C++ library for reading/writing `.ini` files (using custom modified sources).

- [nlohmann/json](https://github.com/nlohmann/json), C++ library for reading/writing JSON files.

- [magic_enum](https://github.com/Neargye/magic_enum), C++ library providing static reflection for enums.

- [boost](https://www.boost.org/), a large C++ library.

- 3ds-libogg and 3ds-libvorbisidec, 3DS portlibs used to decode OGG audio files.
