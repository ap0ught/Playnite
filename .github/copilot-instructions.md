# Copilot Instructions for Playnite

## Project Overview

Playnite is an open source video game library manager and launcher written in C# (.NET/WPF) that supports integration with multiple game libraries (Steam, Epic, GOG, EA App, Battle.net, etc.) and game emulation. The project consists of:

- **Desktop Application** (`Playnite.DesktopApp`) - Full-featured desktop mode
- **Fullscreen Application** (`Playnite.FullscreenApp`) - TV/controller-optimized mode
- **Core Library** (`Playnite`) - Shared business logic and core functionality
- **SDK** (`PlayniteSDK`) - Public API for plugins and extensions
- **Toolbox** (`Playnite.Toolbox`) - Tools for creating extensions and themes

## Important Constraints

⚠️ **Pull Request Policy**: Code contributions (pull requests) are currently **NOT being accepted** while the majority of the code base is being rewritten for Playnite 11. Please wait until P11 is at least in beta state before submitting any pull requests.

## Technology Stack

- **Language**: C# (.NET Framework)
- **UI Framework**: WPF (Windows Presentation Foundation)
- **Build System**: MSBuild via PowerShell scripts
- **Testing**: NUnit, Pester (for integration tests)
- **Localization**: XAML resource files, managed via Crowdin

## Code Style Guidelines

### Naming Conventions

- **Private fields and properties**: Use `camelCase` (without underscore)
  ```csharp
  private string gameName;
  private int gameCount;
  ```

- **All methods** (private and public): Use `PascalCase`
  ```csharp
  private void ProcessGame() { }
  public void StartGame() { }
  ```

- **Public properties and classes**: Use `PascalCase`

### Formatting Rules

- **Indentation**: Use **spaces** instead of tabs with **4 spaces width**
- **Empty lines**: Add an empty line between code block end `}` and additional expressions

  ```csharp
  if (condition)
  {
      DoSomething();
  }

  DoSomethingElse();
  ```

- **Braces**: Always encapsulate code body after `if`, `for`, `foreach`, `while`, etc. with curly braces

  ✅ **Correct**:
  ```csharp
  if (true)
  {
      DoSomething();
  }

  DoSomethingElse();
  ```

  ❌ **Incorrect**:
  ```csharp
  if (true)
      DoSomething();
  DoSomethingElse();
  ```

### EditorConfig

The project uses `.editorconfig` for consistent code formatting. Key diagnostics are suppressed:
- IDE0058: Expression value is never used
- IDE0036: Order modifiers
- IDE0025: Use expression body for properties

## Project Structure

```
Playnite/
├── source/
│   ├── Playnite/              # Core library and business logic
│   ├── Playnite.DesktopApp/   # Desktop mode application
│   ├── Playnite.FullscreenApp/ # Fullscreen mode application
│   ├── PlayniteSDK/           # Public SDK for extensions
│   ├── Tools/                 # Development tools
│   └── Tests/                 # Unit and integration tests
├── build/                     # Build scripts and configuration
├── tests/                     # Integration test scripts
└── .github/                   # GitHub configuration
```

## Development Workflow

### Branches

- `master` - Default branch representing the currently released build
- `devel` - Development branch containing latest changes
- `devel*` - Feature-specific development branches

**All pull requests should be made against the `devel` branch.**

### Building

The project uses PowerShell build scripts:

```powershell
# Build the project
.\build\build.ps1 -Configuration Release -Platform x86

# Build SDK NuGet package
.\build\buildSdkNuget.ps1

# Update localization constants
.\build\buildLocConstants.ps1
```

### Testing

```powershell
# Run integration tests
.\tests\RunTests.ps1

# Run specific test
.\tests\RunTests.ps1 -TestName "Test Name"
```

Unit tests are located in `source/Tests/` and use NUnit.

## Localization

- Localization strings are managed via Crowdin: https://crowdin.com/project/playnite
- **DO NOT MODIFY** `LocalizationKeys.cs` - it's automatically generated via `buildLocConstants.ps1`
- Source localization file: `source/Playnite/Localization/LocSource.xaml`
- English proofreading changes should be submitted as PRs to `LocSource.xaml`

## Extensions and Plugins

Playnite supports:
- **.NET Plugins** - Written in C# or other .NET languages
- **PowerShell Scripts** - For simpler extensions
- **Themes** - Custom UI themes for Desktop and Fullscreen modes

Extension templates are located in `source/Tools/Playnite.Toolbox/Templates/`.

See the [extensions portal](https://api.playnite.link/docs/tutorials/index.html) for documentation.

## Common Patterns

### Logging

```csharp
using Playnite.SDK;

var logger = LogManager.GetLogger();
logger.Info("Information message");
logger.Error(ex, "Error message");
```

### Settings Management

Settings are managed through `PlayniteSettings` class with automatic serialization to JSON.

### Resource Management

- Always dispose of resources properly
- Use `using` statements for `IDisposable` objects
- Be mindful of WPF dispatcher threading

## Important Files

- **Do not modify**: 
  - `source/Playnite/Localization/LocalizationKeys.cs` (auto-generated)
  - Files with "DO NOT MODIFY" header comments
  
- **Build configuration**:
  - `build/build.ps1` - Main build script
  - `source/Playnite.sln` - Main solution file
  - `appveyor.yml` - CI configuration

## Documentation and Support

- **Wiki**: https://github.com/JosefNemec/Playnite/wiki
- **API Documentation**: https://playnite.link/docs/
- **Discord**: https://playnite.link/discord
- **Reddit**: https://www.reddit.com/r/playnite/

## License

MIT License - Copyright (c) 2020 Josef Nemec

## Additional Notes

- The project is currently being rewritten for version 11 in a private repository
- Windows 10 or 11 is required
- Be mindful of the Windows-specific nature of the codebase (WPF, Windows APIs)
- When working with UI, consider both Desktop and Fullscreen modes
- Extension APIs must maintain backward compatibility
