# EasyScripts - Sequential Macro Editor
C# Avalonia (.NET10 / Avalonia 12)
  - Targets Windows, MacOS, and Linux

I plan to make this a commercial software, as I need side income as my current job salary is insufficient for housing.
To create a commercial macro in a market full of free macro software, this software idea needs to deliver something unique, appealing, and useful.

# Purpose
<img width="1243" height="798" alt="image" src="https://github.com/user-attachments/assets/2a4c68b0-60a2-4948-a013-f15edb8f6dfb" />

- Macro steps are sequential
- Macro can be executed "concurrently" (low-level hooks do not support true concurrent actions, so in code, it simply executes from Lane 1 -> Lane 2 -> etc. if both macro steps in both lanes are to be executed in the same timespan)

TODO:
- Create asynchronous monitoring. For example: Lane 1 detects a pixel change, switches to Lane 3. Conditional block with IF -> GOTO, and ELSE -> GOTO
- Create pixel detection (pixel by point, pixel by bitmap)

# PROJECT ARCHITECTURE
<img width="355" height="426" alt="image" src="https://github.com/user-attachments/assets/23d2315e-14bb-4040-ac2b-9736bc3ec615" />

AppHost is composition root. References all other CSPROJ but is not referenced by any itself.
Reference order:
1. AppHost references all
2. UI references Core
3. Infrastructure references Core
4. Platforms references Core
5. Core references no one

AppHost only contains the Program.cs and appsettings.json to be bundled to assembly. Appsettings.json also writes to %LOCALAPPDATA% for user copy
Core contains all abstractions (interfaces), all exceptions, all data and models.
UI contains all UI (Views, ViewModels, Controls (UserControls), Converters) and assets related (Accents, Themes, Styles AXAML)
Infrastructure contains all IO (AppSettingsProvider (singleton wired up to DI container accessible), AppInfo, AppPaths, LogsHandler)
Platforms contains LL hooks to keyboard, mouse, window, and screen capture specific to Windows, Linux, MacOS (IN PROGRESS)

All macro steps uses an execution interface in the format of ValueTask<..> RunAsync(.. ctx). A major refactor as previously each macro step is its own class (as dto class block)


