# Installation Guide

Get SnapThink running on your system in just a few steps.

## System Requirements

### Minimum Requirements
- **RAM**: 8GB (16GB recommended for larger models)
- **Storage**: 5GB free space (plus space for AI models)
- **OS**: macOS 10.15+, Windows 10+, or Ubuntu 18.04+

### Recommended Setup
- **RAM**: 16GB or more
- **Storage**: 20GB+ free space
- **GPU**: Optional but improves AI model performance

## Download SnapThink

### Option 1: Download Pre-built Releases (Recommended)

1. Visit the [SnapThink Releases](https://github.com/snapthinkllm/SnapThinkLibrary/releases) page
2. Download the version for your operating system:
   - **macOS**: `SnapThink-macos.dmg` or `SnapThink-macos.zip`
   - **Windows**: `SnapThink-windows.exe` or `SnapThink-windows-portable.exe`
   - **Linux**: `SnapThink-linux.AppImage`

### Option 2: Build from Source

```bash
# Clone the repository
git clone https://github.com/snapthinkllm/SnapThinkLibrary.git
cd SnapThinkLibrary

# Install dependencies
npm install

# Build and package
npm run make
```

## Installation Steps

### macOS Installation

1. **Download** the `.dmg` file
2. **Open** the downloaded file
3. **Drag** SnapThink to your Applications folder
4. **Launch** SnapThink from Applications
5. If you see a security warning, go to **System Preferences > Security & Privacy** and click "Open Anyway"

### Windows Installation

#### Option A: Installer (NSIS)
1. **Download** the `.exe` installer
2. **Run** the installer as administrator
3. **Follow** the installation wizard
4. **Launch** SnapThink from the Start Menu

#### Option B: Portable
1. **Download** the portable `.exe` file
2. **Create** a folder for SnapThink (e.g., `C:\SnapThink`)
3. **Move** the portable exe to this folder
4. **Run** the executable directly

### Linux Installation

1. **Download** the `.AppImage` file
2. **Make it executable**:
   ```bash
   chmod +x SnapThink-linux.AppImage
   ```
3. **Run** the AppImage:
   ```bash
   ./SnapThink-linux.AppImage
   ```

## Post-Installation Setup

### 1. Install Ollama (Required for AI Features)

SnapThink uses Ollama to run AI models locally.

#### macOS/Linux:
```bash
curl -fsSL https://ollama.com/install.sh | sh
```

#### Windows:
1. Download Ollama from [ollama.com](https://ollama.com)
2. Run the installer
3. Restart your computer

### 2. Download Your First AI Model

After installing Ollama, download a model:

```bash
# Lightweight model (good for most uses)
ollama pull llama3.2:3b

# More powerful model (requires more RAM)
ollama pull llama3.2:8b

# Coding-focused model
ollama pull codellama:7b
```

### 3. Verify Installation

1. **Launch SnapThink**
2. **Create a new notebook**
3. **Type a message** to test the AI chat
4. If everything works, you're ready to go! 🎉

## Troubleshooting

### "No models found" Error
- Make sure Ollama is installed and running
- Download at least one model using `ollama pull`
- Restart SnapThink

### Permission Errors (macOS)
- Right-click SnapThink and select "Open"
- Go to System Preferences > Security & Privacy
- Click "Open Anyway" when prompted

### Antivirus Issues (Windows)
- Add SnapThink to your antivirus exclusions
- Some antivirus software may flag Electron apps

### Linux AppImage Issues
- Make sure the file is executable: `chmod +x SnapThink.AppImage`
- Install FUSE if needed: `sudo apt install fuse`

## Next Steps

Once installed, check out:
- [First Steps Guide](first-steps.md) - Create your first notebook
- [AI Chat Features](../features/ai-chat.md) - Learn about AI capabilities
- [Document Analysis](../features/document-analysis.md) - Upload and analyze files

Need help? Check our [Troubleshooting Guide](../guides/troubleshooting.md).
