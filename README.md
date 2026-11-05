# Box

A data-driven 2D/3D game framework built on [Flecs](https://github.com/SanderMertens/flecs)
and WebGPU.

## Prerequisites

- [bake3](https://github.com/SanderMertens/bake3)
- flecs checked out at `../../flecs` on branch `script_authoring_improvements` and built once
  with `bake3 ../../flecs`. The scenes use script features (`T[N]` variables, `+=`, `a[i] = e`,
  `proc`) that `master` does not have yet.
- A Rust toolchain

## Build

```sh
bake3 --local-env
```

## Run

```sh
bake3 run apps/boxrun --local-env -- etc/scenes/town.flecs
```

## Test

```sh
bake3 run test/cpu/core --local-env -- -j 8
python3 tools/integration.py visual
```

## Docs

- [Audio](docs/audio.md): the guide to sound and music in box, from a first sound to a mixed
  song on the timeline, the web rules, captures and the reference of the audio core, the mixer
  and the sequencer.
- [Curves](docs/curves.md): the guide to authoring curves for particles, lights, emission,
  animation, paths and eases, with recipes, the errors and the cost.

## Commands

A running app takes shell commands on its REST port (`--port`, `PUT /shell/session`), in the
in game console (the grave key) and, in the browser, through
`Module.cwrap('box_command_web_run', 'string', ['string'])`. `help` lists them all. Besides the
builtins of the flecs shell (`ls`, `get`, `set`, `query`, ...):

| Command | What it does |
|---------|--------------|
| `capture [camera]` | Write a PNG of the active camera or a camera, as its view shows it |
| `render <subject>` | Write a PNG of an entity, prefab or template on its own |
| `fit [camera]` | Move the active camera or a camera so that it sees the whole scene |
| `audio` | Print the audio state, rate, voices, load and the buses in processing order |
| `audio volume <v>` | Set `AudioSettings.master_volume`, 0 to 4 |
| `audio mute [on\|off]` | Set or toggle `AudioSettings.mute` |
| `audio capture <seconds>` | Record the master output to a 16 bit stereo WAV resource |
| `fps [rate]`, `frames` | Toggle the fps panel or cap the rate, print the frame timing |
| `edit [on\|off]` | Open or close the editor |
| `logs [level] [count]` | Show the last log messages |
| `key_*`, `mouse_*`, `action_*` | Send input, see `help` |

`capture` of a camera that a view renders shows it as on screen, with the debug drawing of the
view. `capture` and `audio capture` return a resource at once and fill it when the frames are in;
fetch it with `GET /shell/session/<id>/resource/<name>`, which is empty until then. In the
browser, which has no REST port, `audio capture` writes `/tmp/audio_capture_<n>.wav` in the file
system of the page and returns that path. Headless with a fixed `--delta-time`, two runs capture
identical audio, so a render can be listened to and compared. See [docs/audio.md](docs/audio.md).

## Screenshots
<img width="1600" height="973" alt="image" src="https://github.com/user-attachments/assets/f79c8f68-4813-4564-88e7-5a14485cdaf8" />
<img width="800" height="504" alt="Screenshot 2026-11-02 at 1 45 28 PM" src="https://github.com/user-attachments/assets/bb85ef3b-0f08-4391-a2cc-d6f01fe4db93" />
<img width="1600" height="997" alt="image" src="https://github.com/user-attachments/assets/5c39170a-b799-4e6f-beda-3de1e1baca12" />
<img width="1600" height="898" alt="image" src="https://github.com/user-attachments/assets/d98127e7-e923-4d96-a2f4-b92f8a3a8113" />
<img width="1600" height="1010" alt="image" src="https://github.com/user-attachments/assets/465b29c5-e6ba-406c-b95b-9787aa8d485b" />
<img width="3966" height="2502" alt="image" src="https://github.com/user-attachments/assets/9d370b09-d7be-4b59-b65e-0c6c4b898a2b" />
<img width="1980" height="1241" alt="Screenshot 2026-10-20 at 12 24 00 AM" src="https://github.com/user-attachments/assets/7384e4e7-15fd-430f-bd1d-d8a79324d61d" />
<img width="1600" height="959" alt="image" src="https://github.com/user-attachments/assets/2b8abc1f-643f-4ac7-a116-97c59f167c65" />
