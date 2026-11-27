# Лабораторная №2 — Команды движения манипулятора (Godot 4.1.1)

Проект: [`software_0b01/`](software_0b01/) · Методичка: [`Лаб_раб_2.pdf`](Лаб_раб_2.pdf) · Общие материалы курса: [`general.pdf`](../general.pdf) · Описание всех лабораторных: [README](../README.md) ([English](../README.en.md))

Нужны **Godot 4.1.1 Mono (.NET)** и **.NET SDK 6.0**. В менеджере проектов нажмите **Import**, затем **Browse** и выберите `project.godot`, нажмите **Import & Edit** (`software_0b01/project.godot`). Сборка — кнопка **Build** справа вверху или **Alt+B**; **F5** запускает `Main.tscn`.

Это чистый шаблон без решений.

- **Цель:** реализовать два вида движения: переброску (PTP, `Joint4`) — интерполяцию обобщённых координат, и линейное движение (LIN, `Linear4`) — интерполяцию положения фланца.
- **Задание 1.** Задать свою последовательность движений трёхосевого робота в `Prefab/Sample/SampleController.cs` (команды `Joint3` и `Linear3`).
- **Задание 2.** Перенести кинематику из ЛР 1 в `Prefab/Task/TaskRobot.cs` (места `// Fix here`), создать `TaskController.cs`, сцену `TaskScene.tscn` и команду `Code/Task/Joint4.cs`.
- **Задание 3.** Создать `Code/Task/Linear4.cs` и добавить линейные движения в `InitCommands`.
- Код `Joint4` / `Linear4` понадобится в ЛР 3.

Та же лабораторная для Godot 4.7.2 — [`godot-4.7.2/task_02`](https://github.com/exdietolog/robo_lab/tree/godot-4.7.2/task_02).
