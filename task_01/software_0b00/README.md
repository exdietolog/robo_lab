# software 0b00

Лабораторная работа №1: модель робота-манипулятора, прямая и обратная кинематика.

## Сборка и запуск

Нужны **Godot 4.7.2 с поддержкой .NET (Mono)** и **.NET 8 SDK**. Проверьте, что `dotnet --list-sdks` показывает версию `8.x`. При первой сборке могут загружаться пакеты с NuGet.org.

1. Импортируйте `software_0b00/project.godot` в Godot 4.7.2 Mono.
2. Нажмите **F5**, чтобы собрать проект и запустить демонстрацию трёхосевого робота (`Main.tscn`).
3. Для четырёхосевого робота откройте `TaskMain.tscn` и нажмите **F6**. Эта сцена добавляет камеру и освещение к `Prefab/Task/TaskScene.tscn`. При запуске робот следует за движущейся целью.

Чтобы проверить кинематику вручную, откройте `Prefab/Task/TaskScene.tscn`. В 3D-редакторе измените положение `TaskTarget` или его поворот вокруг локальной оси Z: робот должен следовать за достижимой целью. Для проверки прямой кинематики временно очистите поле `Target` у `TaskRobotControl`, затем меняйте углы `A`, `B`, `C`, `D` у `TaskRobot`. Верните ссылку на `TaskTarget`, чтобы снова включить обратную кинематику.

## License

Materials are licensed under the MIT license.

## Godot license

```
This project uses Godot Engine, available under the following license:

Copyright (c) 2014-present Godot Engine contributors. Copyright (c) 2007-2014 Juan Linietsky, Ariel Manzur.

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
```
