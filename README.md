#  ENKIDO - Game Store Application

A modern, cross-platform game store application built with Avalonia UI and .NET 10, featuring a stunning cyberpunk-inspired dark theme with neon accents.

![Enkido Interface](https://img.shields.io/badge/Platform-Desktop%20%7C%20Browser-blue)
![.NET](https://img.shields.io/badge/.NET-10.0-purple)
![Avalonia](https://img.shields.io/badge/Avalonia-11.x-red)

##  Features

- **Cross-Platform**: Runs on Windows, macOS, Linux, and Web browsers
- **Modern UI**: Cyberpunk-inspired dark theme with neon cyan and purple accents
- **Real Game Data**: Integration with RAWG API for trending games
- **Responsive Design**: Unified UI across Desktop and Browser platforms
- **High-Quality Graphics**: Game thumbnails, backgrounds, and ratings

##  Project Structure

```
Enkido/
├── Enkido/                    # Shared UI library (Views, ViewModels, Services)
│   ├── Views/                 # XAML views (MainWindow, MainView)
│   ├── ViewModels/            # MVVM ViewModels
│   ├── Models/                # Data models
│   └── Services/              # API services (GameService)
├── Enkido.Desktop/            # Desktop application head
│   └── Enkido.Desktop.csproj
├── Enkido.Browser/            # Browser (WebAssembly) application head
│   ├── AppBundle/             # Static web assets (index.html, main.js)
│   ├── run.ps1                # Automated build & run script
│   └── Enkido.Browser.csproj
└── README.md
```

##  Getting Started

### Prerequisites

- [.NET 10 SDK](https://dotnet.microsoft.com/download/dotnet/10.0) or later
- Windows, macOS, or Linux
- Modern web browser (for Browser version)

### Installation

1. Clone the repository:
```bash
git clone https://github.com/yourusername/enkido.git
cd enkido
```

2. Restore dependencies:
```bash
dotnet restore
```

##  Running the Desktop Version

From the project root directory:

```bash
dotnet run --project Enkido.Desktop/Enkido.Desktop.csproj
```

##  Running the Browser Version

### Option 1: Using the Automated Script (Recommended)

Navigate to the Browser project directory and run the script:

```powershell
cd Enkido.Browser
powershell -ExecutionPolicy Bypass -File .\run.ps1
```

The script automatically:
- Stops any running dotnet processes
- Cleans build artifacts
- Builds the project
- Copies static assets to the output directory
- Launches the development server

### Option 2: Manual Build & Run

```bash
cd Enkido.Browser
dotnet build
dotnet run
```

Then open your browser to: `http://localhost:5235/AppBundle/index.html`

##  UI Architecture

The application uses a **unified UI approach**:
- `MainView.axaml` contains the complete UI definition
- `MainWindow.axaml` (Desktop) hosts `MainView` for consistency
- Both platforms share the same `MainWindowViewModel`

This ensures pixel-perfect consistency between Desktop and Browser versions.

## 🔧 Configuration

### API Key

The application uses the RAWG API for game data. The API key is configured in:
```
Enkido/Services/GameService.cs
```

To use your own API key:
1. Get a free API key from [RAWG.io](https://rawg.io/apidocs)
2. Replace the `ApiKey` constant in `GameService.cs`

##  Technologies Used

- **Framework**: .NET 10
- **UI Framework**: Avalonia UI 11.x
- **MVVM**: CommunityToolkit.Mvvm
- **HTTP Client**: System.Net.Http with JSON Source Generators
- **Browser Runtime**: WebAssembly (WASM)
- **Icons**: Material Icons
- **Fonts**: Inter (via Avalonia.Fonts.Inter)

## 📝 Key Technical Details

### JSON Serialization for Browser

The Browser version uses **Source-Generated JSON Serialization** to work correctly in WebAssembly:
- `GameJsonContext` is defined in `RawgModels.cs`
- This avoids reflection-based serialization issues in AOT/WASM environments

### Build Configuration

The Browser project includes:
- `PublishTrimmed: false` to preserve necessary assemblies
- Custom `AppBundle` directory for static assets
- Automated asset copying via `run.ps1`

##  Troubleshooting

### Browser Version Shows 404

**Problem**: `index.html` not found after running `dotnet run`

**Solution**: Use the `run.ps1` script which automatically copies static assets:
```powershell
powershell -ExecutionPolicy Bypass -File .\run.ps1
```

### Images Not Loading

**Problem**: Game images not appearing in the browser

**Solution**: Ensure the JSON serialization context is being used. Check that `GameService.cs` uses:
```csharp
await _httpClient.GetFromJsonAsync(url, GameJsonContext.Default.RawgResponse);
```

### File Lock Errors

**Problem**: Build fails with "file is locked" errors

**Solution**: The `run.ps1` script handles this automatically by killing dotnet processes before building.

## 📄 License

This project is licensed under the MIT License.

##  Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## 📧 Contact

For questions or support, please open an issue on GitHub.

---


