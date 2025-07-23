# SnapThink Plugin System

SnapThink features a powerful plugin architecture that allows extending the platform with specialized functionality and AI-powered tools.

## Overview

The plugin system enables:

- **AI Integration**: Plugins can interact with LLMs using specialized prompts
- **Notebook-Specific**: Each notebook can have its own set of active plugins
- **Interactive UI**: Plugins can add sidebar components and chat message types
- **Action System**: Plugin responses can include interactive buttons
- **Event-Driven**: Real-time communication between plugins and the main application

## Architecture

### Core Components

1. **PluginManager** (`src/utils/pluginManager.js`)
   - Central registry for all plugins
   - Handles plugin lifecycle management
   - Routes messages between plugins and notebooks

2. **Plugin Interface** 
   - Standard interface all plugins must implement
   - Provides hooks for initialization, LLM interaction, and cleanup

3. **UI Integration**
   - **PluginsPage**: Marketplace-style interface for managing plugins
   - **PluginsSidebar**: Notebook-specific plugin interface
   - **PluginMessage**: Special message type for plugin responses

### Plugin Lifecycle

```
Registration → Initialization → Notebook Addition → Message Processing → Response Handling
```

## Usage

### For Users

#### Managing Plugins

1. **Access Plugin Marketplace**: Click "Plugins" button on the dashboard
2. **Browse Available Plugins**: View all registered plugins with descriptions
3. **Activate Plugins**: Toggle plugins on/off globally
4. **Add to Notebook**: Add active plugins to specific notebooks

#### Using Plugins in Notebooks

1. **Plugin Sidebar**: View and interact with notebook-specific plugins
2. **Chat Integration**: Type natural language commands that plugins can handle
3. **Quick Actions**: Use plugin-specific action buttons
4. **Custom Prompts**: Send targeted messages to specific plugins

### For Developers

#### Creating a Plugin

1. **Implement Plugin Interface**:

```javascript
class MyPlugin {
  constructor() {
    this.name = 'my-plugin';
    this.displayName = 'My Awesome Plugin';
    this.version = '1.0.0';
    this.description = 'Does amazing things';
    this.icon = '⚡';
    this.category = 'Productivity';
  }

  async initialize() {
    // Setup plugin
    return true;
  }

  async handleLLMInteraction(prompt, context) {
    // Process user messages
    return {
      type: 'response',
      response: 'Plugin response',
      actions: [/* action buttons */]
    };
  }

  getInfo() {
    return {
      name: this.name,
      displayName: this.displayName,
      // ... other metadata
    };
  }

  async destroy() {
    // Cleanup
  }
}
```

2. **Register Plugin**:

```javascript
import pluginManager from '../utils/pluginManager';
import MyPlugin from './my-plugin';

const myPlugin = new MyPlugin();
pluginManager.registerPlugin(myPlugin);
```

#### Plugin Interface Methods

##### Required Methods

- **`initialize()`**: Setup plugin, return true if successful
- **`handleLLMInteraction(prompt, context)`**: Process user messages
- **`getInfo()`**: Return plugin metadata
- **`destroy()`**: Cleanup when plugin is deactivated

##### Optional Methods

- **`getUIComponents()`**: Define custom UI components
- **`on(event, callback)`**: Event system for plugin communication
- **`callLLM(prompt, model)`**: Direct LLM integration

#### Response Format

Plugins can return different response types:

```javascript
// Code generation response
{
  type: 'code-generation',
  response: 'Generated Python code for robot navigation',
  controller: { id: 'ctrl_123', code: '...' },
  actions: [
    { label: 'Deploy Code', action: 'deploy', data: {...} }
  ]
}

// Simulation response
{
  type: 'simulation',
  response: 'Launching Webots simulation...',
  iframe: '<iframe>...</iframe>',
  actions: [
    { label: 'Open Full Screen', action: 'fullscreen', data: {...} }
  ]
}

// General response
{
  type: 'general',
  response: 'Here is the information you requested...',
  actions: [
    { label: 'Learn More', action: 'open-docs', data: {...} }
  ]
}
```

## Built-in Plugins

### Webots Robotics Plugin

- **Purpose**: AI-powered robotics simulation and programming
- **Features**: Code generation, simulation control, Webots.cloud integration
- **Commands**: "generate robot controller", "launch simulation", etc.

## Integration Points

### LLM System

Plugins integrate with SnapThink's LLM system:

```javascript
async handleLLMInteraction(prompt, context) {
  const systemPrompt = this.buildSystemPrompt(context);
  const response = await this.callLLM(`${systemPrompt}\n\n${prompt}`);
  return this.processResponse(response);
}
```

### Chat System

Plugin responses appear as special message types:

- Custom styling and layout
- Interactive action buttons
- Code syntax highlighting
- Embedded media support

### Event System

Plugins can communicate via events:

```javascript
// Listen for events
plugin.on('simulation-started', (data) => {
  console.log('Simulation started:', data);
});

// Emit events
plugin.emit('code-generated', { controllerId: 'abc123' });
```

## Plugin Development Best Practices

### 1. User Experience

- **Clear descriptions**: Write helpful plugin descriptions
- **Intuitive commands**: Support natural language interactions
- **Progressive disclosure**: Start simple, offer advanced features
- **Feedback**: Provide clear status messages and error handling

### 2. Performance

- **Lazy loading**: Only load resources when needed
- **Async operations**: Use async/await for long-running tasks
- **Memory management**: Clean up resources in destroy()
- **Efficient rendering**: Minimize UI re-renders

### 3. Integration

- **Follow conventions**: Use standard response formats
- **Error handling**: Gracefully handle failures
- **Context awareness**: Use notebook and user context appropriately
- **Testing**: Test plugin with different LLM models and scenarios

### 4. Security

- **Input validation**: Sanitize user inputs
- **Sandboxing**: Isolate plugin operations when possible
- **Permissions**: Request minimal necessary permissions
- **Safe defaults**: Use secure default configurations

## Examples

### Simple Echo Plugin

```javascript
class EchoPlugin {
  constructor() {
    this.name = 'echo';
    this.displayName = 'Echo Plugin';
    this.description = 'Echoes back user messages';
    this.icon = '🔄';
  }

  async initialize() {
    return true;
  }

  async handleLLMInteraction(prompt, context) {
    return {
      type: 'general',
      response: `You said: "${prompt}"`,
      actions: [
        { label: 'Say Again', action: 'repeat', data: { message: prompt } }
      ]
    };
  }

  getInfo() {
    return {
      name: this.name,
      displayName: this.displayName,
      description: this.description,
      icon: this.icon,
      isActive: true
    };
  }

  async destroy() {
    // No cleanup needed
  }
}
```

### Calculator Plugin

```javascript
class CalculatorPlugin {
  constructor() {
    this.name = 'calculator';
    this.displayName = 'Calculator';
    this.description = 'Performs mathematical calculations';
    this.icon = '🧮';
  }

  async handleLLMInteraction(prompt, context) {
    const calculation = this.extractCalculation(prompt);
    
    if (calculation) {
      const result = this.calculate(calculation);
      return {
        type: 'calculation',
        response: `${calculation} = ${result}`,
        actions: [
          { label: 'Copy Result', action: 'copy', data: { value: result } }
        ]
      };
    }

    return {
      type: 'general',
      response: 'I can help you with mathematical calculations. Try asking me to solve an equation!',
      actions: []
    };
  }

  extractCalculation(prompt) {
    // Simple regex to extract math expressions
    const match = prompt.match(/([0-9+\-*/().\s]+)/);
    return match ? match[1].trim() : null;
  }

  calculate(expression) {
    try {
      // WARNING: In production, use a safe math parser
      return eval(expression);
    } catch (error) {
      return 'Error: Invalid expression';
    }
  }
}
```

## Future Enhancements

- [ ] Plugin marketplace with community plugins
- [ ] Plugin sandboxing and security improvements  
- [ ] Plugin composition and workflows
- [ ] Cross-plugin communication
- [ ] Plugin analytics and usage metrics
- [ ] Hot reloading for development
- [ ] Plugin templates and scaffolding tools
