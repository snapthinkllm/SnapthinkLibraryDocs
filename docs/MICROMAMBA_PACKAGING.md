# SnapThink Executable Packaging with Micromamba

This document explains how micromamba is packaged when building SnapThink as an executable.

## 📦 Packaging Overview

When you build SnapThink using `npm run make`, here's how micromamba is packaged:

### 1. **Binary Preparation**
- Micromamba binaries (~25-30MB each) are stored in `main/micromamba-bin/`
- Platform-specific subdirectories:
  - `win32-x64/micromamba.exe` (Windows 64-bit)
  - `darwin-x64/micromamba` (macOS Intel)
  - `darwin-arm64/micromamba` (macOS Apple Silicon) 
  - `linux-x64/micromamba` (Linux 64-bit)

### 2. **Smart Packaging Strategy**

#### Development Mode
- All platform binaries are available for cross-platform development
- Code detects `app.isPackaged = false` and uses `__dirname/micromamba-bin/`

#### Production Build
- **Only the current platform's binary** is included in the final executable
- Platform filtering in `forge.config.js` excludes other platform binaries
- Code detects `app.isPackaged = true` and uses `process.resourcesPath/micromamba-bin/`

### 3. **Bundle Size Impact**

| Platform | Binary Size | Final App Size Impact |
|----------|-------------|----------------------|
| Windows | ~30 MB | +30 MB to installer |
| macOS Intel | ~25 MB | +25 MB to .dmg |
| macOS ARM64 | ~25 MB | +25 MB to .dmg | 
| Linux | ~25 MB | +25 MB to .deb/.rpm |

## 🔧 Build Process

### Automatic Setup
When you run `npm run make` or `npm run package`, the build automatically:

1. **Downloads binaries** via `scripts/setup-micromamba.js`
2. **Filters platform-specific files** via forge configuration  
3. **Packages only relevant binaries** in the final executable

### Manual Setup
If you need to manually setup binaries:

```bash
# Windows
cd main/micromamba-bin
setup-binaries.bat

# macOS/Linux  
cd main/micromamba-bin
chmod +x setup-binaries.sh
./setup-binaries.sh
```

## 🎯 Runtime Behavior

### Path Resolution
The app automatically detects its execution context:

```javascript
function getMicromambaDir() {
  if (app.isPackaged) {
    // Packaged app: /path/to/app/resources/micromamba-bin
    return path.join(process.resourcesPath, 'micromamba-bin');
  } else {
    // Development: /path/to/project/main/micromamba-bin
    return path.join(__dirname, 'micromamba-bin');
  }
}
```

### Environment Creation
1. User clicks "Python Env" button in SnapThink
2. App creates isolated environment in `~/snapthink/envs/<notebook-id>/`
3. Micromamba binary from packaged resources manages the environment
4. Each notebook gets its own Python environment with packages

## 🚀 Distribution Scenarios

### Scenario 1: Single Platform Distribution
Most common - distribute only for your platform:
- **Windows**: `.exe` installer (~200MB total with micromamba)
- **macOS**: `.dmg` package (~175MB total with micromamba)
- **Linux**: `.deb`/`.rpm` packages (~175MB total with micromamba)

### Scenario 2: Multi-Platform Distribution
If distributing for multiple platforms:
- Build separately on each platform OR
- Use CI/CD (GitHub Actions) to build for all platforms
- Each platform gets optimized bundle with only its binary

### Scenario 3: Portable Distribution  
For portable/USB stick distributions:
- Use `electron-forge make --platform=all` (requires each platform)
- Or include all binaries and detect at runtime (larger but universal)

## 📋 File Structure in Packaged App

```
SnapThink.exe/
├── resources/
│   ├── app.asar                    # Main application code
│   └── micromamba-bin/             # Micromamba binaries
│       └── win32-x64/              # Only current platform
│           └── micromamba.exe      # ~30MB binary
├── SnapThink.exe                   # Electron runtime
└── [other Electron files...]
```

## ⚙️ Configuration Files

### forge.config.js
- **extraResource**: Includes micromamba-bin in package
- **ignore**: Filters out non-current platform binaries
- **Reduces final size** by 75MB+ (excludes 3 other platform binaries)

### package.json
- **Pre-build hooks**: `npm run setup-micromamba` before packaging
- **Cross-platform support**: Handles both Windows and Unix setups

## 🔍 Troubleshooting

### Binary Not Found Error
```
❌ Micromamba binary not found for platform win32-x64
```
**Solution**: Run `npm run setup-micromamba` to download binaries

### Permission Denied (macOS/Linux)
```  
❌ spawn EACCES
```
**Solution**: Binaries automatically made executable during setup

### Large Bundle Size
- **Expected**: ~25-30MB addition per platform
- **Optimization**: Only current platform binary is packaged
- **Alternative**: Consider cloud-based Python environments for smaller bundles

## 🌟 Benefits of This Approach

1. **Zero Dependencies**: Users don't need Python/conda pre-installed
2. **Isolated Environments**: Each notebook gets clean environment  
3. **Cross-Platform**: Works identically on Windows/macOS/Linux
4. **Optimized Size**: Only relevant platform binary included
5. **Offline Capable**: Full Python environment management without internet
6. **Professional**: Enterprise-ready distribution with embedded dependencies

This approach ensures SnapThink users get a complete, self-contained Python environment management experience regardless of their system setup!
