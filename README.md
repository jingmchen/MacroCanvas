# MacroCanvas - Sequential Macro Editor
C# Avalonia (.NET10 / Avalonia 12)
  - Targets Windows, MacOS, and Linux

My motivation was: my friend needed to use a macro software, and he complained that the good ones on the market are too expensive.

This software will be commercial.

To create a commercial macro in a market full of free macro software, this software idea needs to deliver something unique, appealing, and useful.

Thus, this macro software is built from the ground to have the following features:
a. Asynchronous execution. In each lane, macro steps can execute in parallel. Tho in truth, LL hooks do not support true async or multithreading. In effect, it is just executing from lane 1 to lane 2 to etc. But from the user perspective, the feature looks and feels like async.

b. Because the execution is "async", it enables a lot of features that current macro software do not deliver. For example, if Lane 2 is looping forever to execute a series of mouse clicks, keyboard presses, with time delays. Lane 1 is continuously detecting if a pixel in the relative window in X and Y changes. If yes, execute Lane 3 steps before resuming back to Lane 2.

c. Easy drag and drop, like Power Automate. I used Power Automate for my internship, and it feels great and amazing to use. Thus, I planned to make this software low-code 

# Purpose
<img width="1243" height="798" alt="image" src="https://github.com/user-attachments/assets/2a4c68b0-60a2-4948-a013-f15edb8f6dfb" />

Still pending lots of work. Burned out recently so taking a break to work on this.

TODO: (1) Create 

# PROJECT ARCHITECTURE
<img width="355" height="426" alt="image" src="https://github.com/user-attachments/assets/23d2315e-14bb-4040-ac2b-9736bc3ec615" />

Note: I have changed the name again. Still figuring out the best marketable name.

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

Each csproj have its own DI (this IServiceCollection services) which is called by Program.cs which wires it to the Host.Services

Logging uses Serilog with custom configuration that init before Host

Themes is controlled by a service which uses ResourceDictionary from Avalonia to load in Accents and Themes in Assets folder in UI project, and a private pointer field (int) to point to the right "cache" to load new Accents or Themes as per user toggles

MVVM format with clear separation of responsibilities. Models will only contain pure data or business logic. View will only contain views (partial classes with .AXAML) with DataTemplates configured for each UI parts (such as NavBar). ViewModels ties Models and Views together- the bulk of the code behind the UI logic is here. Dumb logic for UI buttons or actions that don't require Models will still remain in Views, such as SaveBtn which do not access anything from Models. Logic that requires Models will use x:DataType="vm.... points to ViewModel" with {Binding: MethodName} for the AXAML button

All macro steps uses an execution interface in the format of ValueTask<..> RunAsync(.. ctx). A major refactor as previously each macro step is its own class (as dto class block - result in major bloat and scalability issue as I plan to add more macro step types) - IN PROGRESS OF REFACTORING
