# FFmpeg Integration for Webots Recording

## Overview

SnapThink now includes full FFmpeg integration for recording Webots simulations. This feature allows users to capture high-quality video recordings of their robotics simulations directly within the SnapThink environment.

## Features

### ✅ What's Implemented

1. **Automatic FFmpeg Detection**
   - Searches common installation paths on Windows, macOS, and Linux
   - Prompts user to install or locate FFmpeg if not found
   - Validates FFmpeg executable before use

2. **Cross-Platform Screen Recording**
   - **Windows**: Uses `gdigrab` to capture specific window by title
   - **macOS**: Uses `avfoundation` for screen capture
   - **Linux**: Uses `x11grab` for X11 display capture

3. **Quality Settings**
   - **High Quality**: CRF 18, slow preset
   - **Medium Quality**: CRF 23, medium preset (default)
   - **Low Quality**: CRF 28, fast preset

4. **Smart Window Detection**
   - Automatically detects Webots windows on Windows systems
   - Uses PowerShell to enumerate running applications
   - Filters for Webots-related processes and windows

5. **Real-time Recording Management**
   - Start/stop recording with proper process lifecycle management
   - Progress monitoring and status updates
   - Graceful shutdown with SIGTERM signals

6. **Video Output Features**
   - MP4 format with H.264 encoding (libx264)
   - YUV420P pixel format for broad compatibility
   - Configurable frame rates (default: 30 fps)
   - File size and duration reporting

## Installation Requirements

### FFmpeg Installation

#### Windows
1. Download FFmpeg from https://ffmpeg.org/download.html
2. Extract to `C:\ffmpeg\` or `C:\Program Files\ffmpeg\`
3. Add `bin` folder to system PATH

#### macOS
```bash
# Using Homebrew
brew install ffmpeg

# Using MacPorts
sudo port install ffmpeg
```

#### Linux
```bash
# Ubuntu/Debian
sudo apt update && sudo apt install ffmpeg

# CentOS/RHEL
sudo yum install ffmpeg

# Arch Linux
sudo pacman -S ffmpeg
```

## Usage

### Basic Recording

1. **Start Webots Simulation**
   - Select project folder
   - Launch simulation from SnapThink

2. **Begin Recording**
   - Click "🎬 Record" button in Webots plugin
   - FFmpeg will automatically detect the Webots window
   - Recording starts immediately

3. **Stop Recording**
   - Click "⏹️ Stop Recording" button
   - Video file is automatically saved to project's `simulations/` folder
   - File information (duration, size) is displayed

### Advanced Options

```javascript
// Custom recording options
const recordingOptions = {
  outputPath: '/custom/path/to/recordings',
  filename: 'my_simulation.mp4',
  format: 'mp4',
  quality: 'high',    // 'low', 'medium', 'high'
  fps: 60,            // Frame rate
  windowTitle: 'Custom Window Title'
};

await webotsPlugin.startRecording(recordingOptions);
```

## Implementation Details

### FFmpeg Command Structure

#### Windows (gdigrab)
```bash
ffmpeg -f gdigrab -framerate 30 -i "title=Webots" -c:v libx264 -preset medium -crf 23 -pix_fmt yuv420p -y output.mp4
```

#### macOS (avfoundation)
```bash
ffmpeg -f avfoundation -framerate 30 -i 1:0 -c:v libx264 -preset medium -crf 23 -pix_fmt yuv420p -y output.mp4
```

#### Linux (x11grab)
```bash
ffmpeg -f x11grab -framerate 30 -i :0.0 -c:v libx264 -preset medium -crf 23 -pix_fmt yuv420p -y output.mp4
```

### Process Management

1. **Startup**: Spawn FFmpeg process with configured arguments
2. **Monitoring**: Listen for stdout/stderr for progress updates
3. **Shutdown**: Send 'q' command to FFmpeg stdin for graceful termination
4. **Cleanup**: Remove process from tracking map after completion

### Error Handling

- **FFmpeg Not Found**: Prompts user with download/locate options
- **Invalid Window**: Falls back to default capture methods
- **Recording Conflicts**: Prevents multiple simultaneous recordings
- **Process Errors**: Captures and reports FFmpeg error messages

## File Organization

```
project/
├── simulations/           # Recorded videos
│   ├── simulation_1234567890.mp4
│   ├── simulation_1234567891.mp4
│   └── webots.log        # Webots output log
├── controllers/          # Robot controllers
├── worlds/              # World files
└── README.md
```

## Future Enhancements

### Planned Features

1. **Audio Recording**
   - Capture system audio alongside video
   - Microphone input for narration

2. **Region Selection**
   - Custom screen regions for recording
   - Multi-monitor support

3. **Live Streaming**
   - RTMP streaming to platforms
   - WebRTC for real-time sharing

4. **Post-Processing**
   - Automatic video optimization
   - Thumbnail generation
   - Metadata embedding

5. **Batch Operations**
   - Record multiple simulations automatically
   - Batch conversion and optimization

## Troubleshooting

### Common Issues

**"FFmpeg not found"**
- Install FFmpeg from official website
- Ensure FFmpeg is in system PATH
- Use "Locate FFmpeg" option to manually select executable

**"No Webots window detected"**
- Ensure Webots is running and visible
- Try manually specifying window title
- Check for multiple Webots instances

**"Recording failed to start"**
- Verify FFmpeg installation
- Check file permissions in output directory
- Ensure sufficient disk space

**"Poor video quality"**
- Increase quality setting to "high"
- Adjust frame rate for smoother recording
- Check system performance during recording

### Performance Tips

1. **System Resources**
   - Close unnecessary applications during recording
   - Monitor CPU/GPU usage
   - Ensure adequate RAM availability

2. **Storage Optimization**
   - Use fast SSD for temporary recording files
   - Monitor available disk space
   - Clean up old recordings regularly

3. **Quality vs Size**
   - Use "medium" quality for balanced results
   - Higher frame rates increase file size
   - Consider storage limitations when choosing settings

## API Reference

### Plugin Methods

```javascript
// Check FFmpeg availability
const isAvailable = await plugin.checkFFmpegAvailability();

// Start recording with options
const result = await plugin.startRecording({
  quality: 'high',
  fps: 60,
  filename: 'custom_name.mp4'
});

// Stop current recording
const output = await plugin.stopRecording();

// Get all recordings
const recordings = plugin.getSimulationOutputs();
```

### IPC Handlers

- `webots:check-ffmpeg` - Check FFmpeg availability
- `webots:get-windows` - Get available windows (Windows only)
- `webots:start-recording` - Begin screen recording
- `webots:stop-recording` - End screen recording

### Events

- `webots:recording-progress` - Real-time recording progress
- `webots:recording-finished` - Recording completion notification
- `recording-started` - Plugin-level recording start event
- `recording-stopped` - Plugin-level recording stop event

---

This implementation provides a solid foundation for video recording in SnapThink's Webots integration, with room for future enhancements and optimizations.
