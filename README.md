[![Build Status](https://travis-ci.org/tsoding/nothing.svg?branch=master)](https://travis-ci.org/tsoding/nothing)

# Nothing

![](https://i.imgur.com/7mECYKU.gif)
![](https://i.imgur.com/ABcJqB5.gif)

## Dependencies

- [gcc]
- [cmake]
- [python3]
- [libsdl2-dev]
- [libsdl2-mixer-dev]
- [inotify-tools]

### Ubuntu

```console
$ sudo apt-get install gcc cmake libsdl2-dev libsdl2-mixer-dev python3 inotify-tools
```

### MacOS

```console
$ brew install gcc cmake sdl2 sdl2_mixer python3
```

### NixOS

For [NixOS] we have a development environment defined in [default.nix]
with all of the required dependencies. You can enter the environment
with `nix-shell` command:

```console
$ nix-shell
```

### Arch Linux

```console
$ sudo pacman -S gcc cmake sdl2 sdl2_mixer python inotify-tools
```

### Windows

See [Build on Windows][build-on-windows] section.

## Quick Start

```console
$ mkdir build
$ cd build/
$ cmake ..
$ make
$ ./nothing ../levels/level-01.txt
$ ./nothing_test
```

## Controls

### Game

#### Keyboard

| Key     | Action                                                      |
|---------|-------------------------------------------------------------|
| `d`     | Move to the right                                           |
| `a`     | Move to the left                                            |
| `SPACE` | Jump                                                        |
| `c`     | Open debug console                                          |
| `r`     | Reload the current level including the Player's position    |
| `q`     | Reload the current level preserving the Player's position   |
| `p`     | Toggle game pause                                           |
| `l`     | Toggle transparency on objects. Useful for debugging levels |

#### Gamepad

| Button       | Action                 |
|--------------|------------------------|
| `Left Stick` | Movement of the Player |
| `1`          | Jump                   |

### Consolé

| Key       | Action                   |
|-----------|--------------------------|
| `ESC`     | Exit console             |
| `Enter`   | Evaluate the expression  |
| `Up/Down` | Traverse console history |

## Editing Levels

Generally creating a level looks like:

```
SVG File -> Custom Level File -> Game
```

To convert SVG to the level file and run [svg2rects.py] script:

```console
$ python3 ./devtools/svg2rects.py -i <svg-file> -o <level-file>
```

All of the levels reside in the [./levels/] folder. Use
[./levels/Makefile] to automatically rebuild all levels.

### Level Editing Workflow

1. `$ inkscape ./levels/level.svg &`
2. `$ ./build/nothing ./levels/level.txt &`
3. `$ cd ./levels/`
4. `$ make watch`
5. Edit Level in Inkscape and Save
6. Switch to the Game and reload level by pressing Q
7. Go to 5

### Objects Reference

#### SVG rect node

| Regex of id  | Description                                                                                                       |
|--------------|-------------------------------------------------------------------------------------------------------------------|
| `player`     | Defines the **position** of the Player. **Size is ignored**.                                                      |
| `rect.*`     | Defines the **size** and **position** of an impenetrable platform block                                           |
| `box.*`      | Defines the **size** and **position** of a rigid box that obeys the physics of the game                           |
| `region(.*)` | Defines the **size** and **position** of a region that hides the Goals. `\1` defines the id of the Goal to hide.  |
| `goal(.*)`   | Defines the **position** of the goal. **Size is ignored**. `\1` defines the id of the region that hides the goal. |
| `lava.*`     | Defines the **position** and **size** of a lava block.                                                            |
| `background` | Defines the **color** of the background. **Position and size are ignored**.                                       |
| `backrect.*` | Defines the **size** and **position** of a solid block in the background.                                         |

#### SVG text node

| Regex of id | Description                                                                |
|-------------|----------------------------------------------------------------------------|
| `label.*`   | Defines **position** and **text** of a in-game label. **Size is ignored**. |

## Build on Windows

You need to install [conan][] and [Visual Studio 2017][visual-studio].

### Dependencies

Current version of [SDL2/2.0.5@lasote/stable][conan-sdl2] in the conan-transit
repository is not compatible with the latest conan, so you'll need to install
it locally from the git repository:

```console
$ git clone https://github.com/lasote/conan-sdl2.git # temporary, I hope hope hope
$ cd conan-sdl2
$ conan export SDL2/2.0.5@lasote/stable
```

### Building

Execute the following commands inside of the `nothing` directory:

```console
$ conan install --build missing --install-folder build
$ cd build
$ cmake .. -G "Visual Studio 15 2017 Win64"
```

After that, build the `build/nothing.sln` file with Visual Studio.

## Build on Windows - build.bat

1. Download and install [Visual Studio 2017][visual-studio]
1. Download [SDL2 Development Lib][sdl2_dev_lib_win] and [SDL2 Mixer Development Lib][sdl2_mixer_dev_lib_win]
2. Inside the SDL2 zip:
    * extract the content of `include` folder in a directory with the following structure: C:\\Dev\\include\\SDL2\\*.h
    * extract the content of `lib\x64` folder in directory with the following structure: C:\\Dev\\lib\\
3. Inside the SDL2 Mixer zip:
    * extract the content of `include` folder in a directory with the following structure: C:\\Dev\\include\\SDL2\\*.h
    * extract the content of `lib\x64` folder in directory with the following structure: C:\\Dev\\lib\\
4. Inside the `nothing` directory edit the `build.bat` file and specify where the headers and libs for SDL2 and SDL2 Mixer are

### Building

Execute the following commands inside of the `nothing` directory:

```console
> %comspec% /k "C:\Program Files (x86)\Microsoft Visual Studio\2017\Community\VC\Auxiliary\Build\vcvars64.bat"
> build.bat
```
It will build, inside `win32_build` folder, the main executable, `nothing.exe`, plus repl.exe and nothing_test.exe.
If everything went wright type

```console
> win32_build\nothing.exe levels\level-01.txt
```

## Support

You can support my work via

- Twitch channel: https://www.twitch.tv/subs/tsoding
- Patreon: https://www.patreon.com/tsoding

[conan]: https://www.conan.io/
[conan-sdl2]: https://bintray.com/conan/conan-transit/SDL2%3Alasote/2.0.5%3Astable
[visual-studio]: https://www.visualstudio.com/
[sdl2_dev_lib_win]: https://www.libsdl.org/release/SDL2-devel-2.0.9-VC.zip
[sdl2_mixer_dev_lib_win]: https://www.libsdl.org/projects/SDL_mixer/release/SDL2_mixer-devel-2.0.4-VC.zip
[svg2rects.py]: ./devtools/svg2rects.py
[./levels/]: ./levels/
[./levels/Makefile]: ./levels/Makefile
[gcc]: https://gcc.gnu.org/
[cmake]: https://cmake.org/
[libsdl2-dev]: https://www.libsdl.org/
[libsdl2-mixer-dev]: https://www.libsdl.org/projects/SDL_mixer/
[python3]: https://www.python.org/
[NixOS]: https://nixos.org/
[default.nix]: ./default.nix
[build-on-windows]: #build-on-windows
[inotify-tools]: https://github.com/rvoicilas/inotify-tools
