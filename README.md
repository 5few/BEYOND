# BEYOND — Fabric 1.21.11 — v0.3.0-alpha (source)

This is the merged **test prototype** of the existing v0.2 tutorial and a playable Observatory → Arboria gateway loop.

**This ZIP is NOT an installable mod JAR.** A production build has not yet been compiled or tested against the Fabric game runtime in this environment.

## Requirements

- Minecraft Java Edition 1.21.11
- Fabric Loader 0.18.4 (or compatible)
- Fabric API for 1.21.11
- Java 21 for building
- Optional: Essential for inviting friends to a singleplayer world (both players need matching mods)
- For `/BEYOND`, host must have permission/cheats/OP privileges

## Newly added in v0.3

- Existing moderator-only `/BEYOND start`, `stop`, `status` command (both letter cases registered)
- Existing O key player menu, `beyond_join`, Korean polite tutorial intro, movement and look training
- Tutorial completion persisted in the world's `beyond-progress.properties`
- A **separate** `beyond:observatory` flat dimension. Building the hub does not overwrite the player's existing Overworld structures.
- A **separate** `beyond:arboria` dimension. The initial biome is a *vanilla forest placeholder*, not yet the planned unique Arboria world generator.
- The observatory structure with control pedestal and walk-through gate, procedurally placed in its own dimension
- A client-only dimension-selection screen with Arboria selectable and secret dimensions locked
- Portal walking collision on the *server* that teleports into Arboria
- A moss landing with stone root gate for return to observatory
- A short first-arrival Echo text sequence (not a real cinematic camera yet)
- Original position saved when leaving the normal world and a return-to-original-world option
- Server-authoritative permission/proximity checks

## How to try

1. Install Fabric 1.21.11 and Fabric API, build this project, then add the resulting `beyond-fabric-0.3.0-alpha.jar` to `.minecraft/mods`.
2. Make a **backup** of a test world before trying. Prefer a **new test world**, because custom dimensions are registered during world loading.
3. Host with cheats enabled, type `/BEYOND start` (admin only).
4. Press **O** → **모험 참여 / 튜토리얼**. Watch 3 short text narration lines and then move horizontally ~4 blocks; look ~60° to finish the short alpha tutorial.
5. Press **O** → **중앙 관측소 이동**. This transfers you to the built Observatory hub.
6. Walk closer to the lectern in front of the gate, press **O** → **차원 관측 지도** → **아르보리아 · 관문 활성화**.
7. **Walk through** the open rectangular gate (towards negative Z) to enter Arboria.
8. At the Arboria arrival circle, walk through the mossy gate to return to the Observatory.
9. Press **O** → **원래 세계로 귀환** to go back to your previously saved position.

The Observatory is intentionally a rough *block prototype*: geometry/lighting, no complete high-quality 81×81 art, custom entity models, or full movie camera yet.

## Keys and multiplayer

- O key is a Minecraft configurable keybind: Settings → Controls → Key Binds → BEYOND.
- **Full composite-key controls are NOT implemented in this alpha**. They remain a planned client feature.
- The moderator `/BEYOND` command is OP-only, while O-menu and the internal `beyond_join` / `beyond_gate` commands are player actions with server-side checks.
- This project has no Essential-specific API dependency. Essential hosting should behave as an integrated-server game, **but interoperability is not yet verified**.
- The first-arrival text sequence runs for each player independently. Multiplayer camera synchronization has not been implemented.

## Build

### Easiest: GitHub Actions

1. Create a GitHub repository and upload **all files** from this project (including `.github/workflows/build.yml`).
2. In Actions, select `Build BEYOND Fabric 1.21.11`, click **Run workflow**.
3. If the build succeeds, download the artifact and extract `beyond-fabric-0.3.0-alpha.jar`.
4. The JAR goes into `.minecraft/mods`, **not** the Paper `plugins` folder.
5. If it fails, copy the `Build and remap for production` failure log for debugging.

### Local Gradle build

Install Java 21 and Gradle 9.2.1 (or compatible). In the project root run:

```bash
gradle clean build --no-daemon --stacktrace
```

Expected remapped production output:

```
build/libs/beyond-fabric-0.3.0-alpha.jar
```

The project intentionally has no Gradle wrapper binary; the workflow installs Gradle for you.

## Not yet included

- Actual film-style moving camera, narrated cutscene animations, BGM synchronization
- Custom world-height extension (currently uses the vanilla Overworld dimension type for both test dimensions)
- Arboria-only custom foliage, world tree, ecosystems, city, 10 story quests, bosses
- Full NPC models, real Echo companion, Waguri, other dimensions
- Flexible Shift+right-click / composite key binding, advanced HUD, proper multiplayer party UI
- A fully polished observatory structure, collision-safe intro cutscene, survival balancing

## Data safety limitations

- Do not delete the dimension files from an existing world that depends on them without a backup.
- A first-time hub visit captures a return position; it is preserved across restarts in `beyond-progress.properties`.
- The procedural Observatory and Arboria landing alter only their **own new dimensions**.
- The code is a first pass. Until tested in a real Fabric client, crashes or world-generation mistakes are possible.

## Project code layout

- `dev.beyond.core.BeyondMod` commands and lifecycle
- `dev.beyond.core.Sessions` tutorial loop
- `dev.beyond.core.ProgressStore` saved tutorial and return positions
- `dev.beyond.core.GateSystem` generated set-pieces and real dimension teleports
- `dev.beyond.client.BeyondClient` O keybind
- `dev.beyond.client.BeyondMenu` player menu
- `dev.beyond.client.DimensionMapScreen` dimension UI
- `data/beyond/dimension/*.json` custom dimension definitions
