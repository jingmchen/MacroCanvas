# EasyScripts - Sequential Macro Editor

C# Avalonia (.NET10 / Avalonia 12)
  - Targets Windows, MacOS, and Linux

Multiproject setup:
  - EasyScripts.AppHost
    -> Composition root. Setup Serilog, wire up services via DI, and set up Host
  - EasyScripts.UI
    -> Frontend of the application. Contains Views, ViewModels, Controls (DataTemplates, Dto UserControls)
  - EasyScripts.Core
    -> Business logic of the application. Contains Interfaces, Dtos, Serialization, etc.
  - EasyScripts.Platform.Windows / Platform.MacOS / Platform.Linux
    -> Output type as Library, contains Native methods for key and mouse hooks for selected platform
  

# Purpose

I needed a macro software that could perform beyond what TinyTasks and other free macro software could offer:
  - OCR or image capture
  - Conditional logic; If window shows X instead of Y, proceed to branch 1, otherwise proceed to branch 2
  - Async running of multiple macro sequences in parallel
  - Scheduling


# Current status

- AppHost
  -> Completed
- UI
  -> Wiring up EditorWindow and all the required DataTemplates for the View
  -> Aiming to make a modern UI similar to RPA automation tools
- Core
  -> Wiring up in conjunction with UI
  -> Currently included: Mouse steps, Key steps, Power shell steps, Pixel Color check steps, Image (bitmap) check steps, Conditional steps
  -> More on the way as I am still thinking on how to expand this further

# Screenshots
<img width="1508" height="902" alt="image" src="https://github.com/user-attachments/assets/ddcf1dfb-4788-4e00-8e95-06abe33e30c6" />


