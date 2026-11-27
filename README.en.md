# robo_lab — Godot 4.7.2

[Русская версия](README.md) · other version: branch [`godot-4.1.1`](https://github.com/exdietolog/robo_lab/tree/godot-4.1.1)

Lab works for the course "Programming of mechatronic and robotic systems" (MSUT STANKIN, Department of Robotics and Mechatronics). All labs are done in the **Godot** engine in **C#**: you build a manipulator model, solve its kinematics and write motion programs.

This is the **`godot-4.7.2`** branch: all four labs are clean templates (no solutions) for **Godot 4.7.2 .NET / .NET 8.0** (`net8.0`). The same labs for Godot 4.1.1 are in the [`godot-4.1.1`](https://github.com/exdietolog/robo_lab/tree/godot-4.1.1) branch.

## Requirements

| What | Version | Download |
|---|---|---|
| Godot with .NET support | 4.7.2 (.NET, not the standard build) | [godotengine.org](https://godotengine.org/download/archive/4.7.2-stable/) |
| .NET SDK | 8.0 or newer (10.0 works too) | [dotnet.microsoft.com](https://dotnet.microsoft.com/download/dotnet/8.0) |
| Code editor (optional) | VS Code with [godot-tools](https://marketplace.visualstudio.com/items?itemName=geequlim.godot-tools) and [C#](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp) extensions, or Visual Studio | — |

Check the SDK with `dotnet --list-sdks`. The standard (non-.NET) Godot build cannot open these projects: the C# scripts will not be found.

## Opening a lab

1. Start Godot and in the Project Manager click **Import**, pick `project.godot`, keep **Edit Now** checked and click **Import** (for example `task_03/software_0b10/project.godot`).
2. Build the C# code: the **Build Project** button (hammer, top-right) or **Alt+B**. The first build downloads Godot packages from NuGet, so internet access is required. Build errors are shown in the **MSBuild** bottom panel.
3. **F5** runs the main scene, **F6** runs the scene currently open.

Without the editor: `godot --path task_03/software_0b10` (run) or `dotnet build "software 0b10.csproj"` in the project folder (build only).

## Layout

| Path | Contents |
|---|---|
| [`general.pdf`](general.pdf) | general course materials (Russian): tools, links to the labs |
| [`task_01/`](task_01/) | Lab 1: `software_0b00` template, handout [`Лаб_раб_1.pdf`](task_01/Лаб_раб_1.pdf) |
| [`task_02/`](task_02/) | Lab 2: `software_0b01` template, handout [`Лаб_раб_2.pdf`](task_02/Лаб_раб_2.pdf) |
| [`task_03/`](task_03/) | Lab 3: `software_0b10` template, handout [`Лаб_раб_3.pdf`](task_03/Лаб_раб_3.pdf) |
| [`task_04/`](task_04/) | Lab 4: `software_0b11` template, handout [`Лаб_раб_4.pdf`](task_04/Лаб_раб_4.pdf) |

## Labs

All labs use the same robot — a four-axis manipulator (column rotation `A`, shoulder `B`, forearm `C`, flange rotation `D`). The flange pose is the `Pose4` struct: point `translation` (m) and rotation about the vertical axis `rotation` (degrees). The robot implements `IMechanism<Gen, Pos>`: forward kinematics is `SolveForward`, inverse is `SolveInverse`.

### Lab 1. Manipulator modelling, forward and inverse position kinematics

Folder [`task_01`](task_01/), handout [`Лаб_раб_1.pdf`](task_01/Лаб_раб_1.pdf) (Russian), project `software_0b00`.

- **Goal:** assemble a robot model from ready-made parts and solve forward and inverse position kinematics for it.
- **Task 1.** Build the scene `Prefab/Task/TaskRobot.tscn` from the models in `Resources/Models/Task/`, place the `Node3D` nodes using the dimensions from the handout, fill the node references of the `TaskRobot` script in the Inspector and make the model move when `A`–`D` change (`_Process`). The script is marked `[Tool]`, so motion can be checked right in the editor: rebuild (Alt+B) and change `A`–`D` in the Inspector.
- **Task 2.** Forward kinematics: `SolveForward` in `TaskRobot.cs`. Check: the robot holds the `Hold` object correctly in `TaskScene.tscn`.
- **Task 3.** Inverse kinematics: `SolveInverse`. Check: the robot follows `TaskTarget` and rotates the flange with it.
- **Example:** the three-axis robot `Prefab/Sample/SampleRobot.cs` with both problems solved — start from it. `Prefab/Task/` is empty in the template: the four-axis robot scenes and scripts are created following the handout.

### Lab 2. Manipulator motion commands

Folder [`task_02`](task_02/), handout [`Лаб_раб_2.pdf`](task_02/Лаб_раб_2.pdf) (Russian), project `software_0b01`.

- **Goal:** implement two motion types: joint motion (PTP, `Joint4`) — interpolation of generalized coordinates, and linear motion (LIN, `Linear4`) — interpolation of the flange pose.
- **Task 1.** Define your own motion sequence for the three-axis robot in `Prefab/Sample/SampleController.cs` (`Joint3` and `Linear3` commands).
- **Task 2.** Port the kinematics from lab 1 into `Prefab/Task/TaskRobot.cs` (`// Fix here` places), create `TaskController.cs`, the `TaskScene.tscn` scene and the `Code/Task/Joint4.cs` command.
- **Task 3.** Create `Code/Task/Linear4.cs` and add linear moves to `InitCommands`.
- The `Joint4` / `Linear4` code is reused in lab 3.

### Lab 3. Local tool and part frames

Folder [`task_03`](task_03/), handout [`Лаб_раб_3.pdf`](task_03/Лаб_раб_3.pdf) (Russian), project `software_0b10`.

- **Goal:** program the motion of the tool centre point (TCP) instead of the flange, expressed in the part frame.
- **Key formula:** `F_flange = F_part × F_TCP,part × F_TCP,flange⁻¹`. In code: `Combine` multiplies poses, `Inverse` inverts a pose (`Frame<Pos>` interface).
- **Task 1.** Complete `Code/Command/Joint4.cs` (places marked `(1.x)`): port the joint motion from lab 2 and recompute the start and target poses using `tool` and `part`.
- **Task 2.** The same for `Code/Command/Linear4.cs` (`(2.x)`); the TCP pose is interpolated, not the flange. Then uncomment the `(2.3)` block in `Prefab/Task/TaskController.cs`.
- **Task 3.** Write your own motion program relative to the `PartGreen` and `PartRed` parts (already placed in `Prefab/Task/TaskScene.tscn`; the tools are the red and green tips of `DemoTool`).
- While the stubs are empty the robot does not move and the build prints 2 warnings about the unused `progress` field — this is expected.

### Lab 4. Controlling the manipulator with async functions

Folder [`task_04`](task_04/), handout [`Лаб_раб_4.pdf`](task_04/Лаб_раб_4.pdf) (Russian), project `software_0b11`.

- **Goal:** write the robot program as ordinary code with `await`, loops and conditions instead of a pre-filled command queue.
- **The template provides:** `Joint4` / `Linear4` / `WaitFor` commands, the `Gripper`, two feeders `BoxSource` / `BoxSource2` (a box every 15 and 20 s) and a demo `Run` function in `Prefab/TaskController.cs` that stacks two boxes.
- **Task 1.** Rewrite `Run` for your assignment — stack boxes with sequential code (`PickBox` / `PlaceBox`).
- **Task 2.** Put the repeated actions into a loop.
- **Task 3.** In the loop take the first ready box from either feeder: `await WaitFor(() => boxSource.BoxIsReady || boxSource2.BoxIsReady);`
- Waiting for boxes takes a while; speed the simulation up with `godot --path task_04/software_0b11 --time-scale 4`.

## Handouts

The handouts (in Russian) are updated for Godot 4.7.2: button names and versions, plus mistakes of the original handouts (see the table). Edited places use the same fonts; there is a «Редакция 2026 г.» line at the bottom of the first page. Lab 1 screenshots are still from Godot 4.1 — the main panels have not moved.

| Before (4.1) | Now (4.7.2) |
|---|---|
| Godot 4.1 Mono, .NET 6 (`net6.0`) | Godot 4.7.2 .NET, .NET 8 (`net8.0`), `Godot.NET.Sdk/4.7.2` |
| Import: **Browse** → **Import & Edit** | **Import** → pick `project.godot` → **Edit Now** checkbox → **Import** |
| **Build** button | **Build Project** (still Alt+B) |
| Editor modes 2D / 3D / Script / AssetLib | new **Game** tab (the running game embedded in the editor) |
| Project Camera Override button on the 3D toolbar | moved to the **Game** tab (Camera Override); the 3D toolbar has a new **Ruler Mode** |
| "Godot with Mono support" | "with .NET support" |
| Only `.cs` next to scripts | every script has a `.cs.uid` file — commit it together with the script |
| Lab 1, task 1, items 15–17: "set all resource values", "run the build and change the values" | item 16: in the `TaskRobot` Inspector fill the node references (`Column`, `Shoulder`, …) with `Node3D` nodes, not the same-named `MeshInstance3D`; item 17: the script is `[Tool]` and runs in the editor — rebuild (Alt+B) and change `A`–`D` in the Inspector, no need to run the game |
| Lab 2: file `res://Code/Task/Join4.cs` | `res://Code/Task/Joint4.cs` |
| Lab 3: "project software_10", nodes "GreenPart / RedPart" | project `software_0b10`, nodes `PartGreen` / `PartRed` |
| Lab 4: title copied from lab 3, "software_11", `Main.tscn` | title "Controlling the manipulator with async functions", `software_0b11`, `main.tscn` |
| `general.pdf`: typos, links to the red-hara templates | typos fixed, links point to the folders of this branch |

## Troubleshooting

- **".NET Sdk not found" / scripts are not found** — the standard Godot build is running or .NET SDK 8.0 is not installed (`dotnet --list-sdks`). Remove an old 32-bit .NET if you have one.
- **The first build "hangs"** — Godot NuGet packages are being downloaded; nuget.org must be reachable.
- **New `[Export]` fields do not show up in the Inspector** — rebuild (Alt+B) and reopen the scene.
- **Linux / WSL without a GPU** — Godot renders on the CPU (llvmpipe): slow, but it works. You can try `--rendering-driver opengl3`.

## License

The lab materials are MIT-licensed (see `LICENSE` in the project folders); templates by [red-hara](https://github.com/red-hara). Godot Engine is MIT-licensed.
