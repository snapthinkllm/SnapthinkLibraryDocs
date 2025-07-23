# Robotics with Webots

This guide will walk you through integrating Webots robotics simulator with SnapThink to create powerful robotics development workflows.

## Prerequisites

### Install Webots
1. Download Webots from [cyberbotics.com](https://cyberbotics.com/download)
2. Follow installation instructions for your platform:
   - **macOS**: Mount the DMG and drag to Applications
   - **Windows**: Run the installer as administrator
   - **Linux**: Extract and add to PATH

### Verify Installation
```bash
# Check Webots is accessible from command line
webots --version

# Should output something like: Webots R2023b
```

## Quick Start: Your First Robot Simulation

### Step 1: Create a New Notebook
1. Open SnapThink
2. Click "New Document" → "Python Notebook"
3. Name it "My First Robot Simulation"

### Step 2: Set Up Webots Connection
```python
# Initialize Webots connection
import sys
sys.path.append('/Applications/Webots.app/lib/controller/python')  # macOS
# sys.path.append('C:/Program Files/Webots/lib/controller/python')  # Windows
# sys.path.append('/usr/local/webots/lib/controller/python')  # Linux

from controller import Robot, Motor, DistanceSensor

# Create robot instance
robot = Robot()

# Get simulation timestep
timestep = int(robot.getBasicTimeStep())

print("✅ Connected to Webots simulation!")
print(f"Simulation timestep: {timestep}ms")
```

### Step 3: Load a Robot World
```python
# For this example, we'll use the built-in e-puck robot
# In Webots: File → Open World → projects/robots/gctronic/e-puck/worlds/e-puck.wbt

# Initialize robot components
left_motor = robot.getDevice('left wheel motor')
right_motor = robot.getDevice('right wheel motor')

# Set motors to velocity control
left_motor.setPosition(float('inf'))
right_motor.setPosition(float('inf'))
left_motor.setVelocity(0.0)
right_motor.setVelocity(0.0)

# Initialize distance sensors
distance_sensors = []
sensor_names = ['ps0', 'ps1', 'ps2', 'ps3', 'ps4', 'ps5', 'ps6', 'ps7']

for name in sensor_names:
    sensor = robot.getDevice(name)
    sensor.enable(timestep)
    distance_sensors.append(sensor)

print("🤖 Robot initialized with motors and sensors")
```

### Step 4: Implement Basic Behavior
```python
# Simple obstacle avoidance behavior
def run_obstacle_avoidance():
    step_count = 0
    max_steps = 1000  # Run for limited time
    
    # Record data for analysis
    sensor_history = []
    position_history = []
    
    while robot.step(timestep) != -1 and step_count < max_steps:
        # Read sensor values
        sensor_values = [sensor.getValue() for sensor in distance_sensors]
        sensor_history.append(sensor_values.copy())
        
        # Simple obstacle avoidance logic
        left_speed = 6.28  # Base speed
        right_speed = 6.28
        
        # If obstacles detected in front, turn
        if sensor_values[0] > 80 or sensor_values[7] > 80:  # Front-left or front-right
            # Turn right
            left_speed = 6.28
            right_speed = -6.28
        elif sensor_values[1] > 80 or sensor_values[2] > 80:  # Left side
            # Turn right
            left_speed = 6.28
            right_speed = 3.14
        elif sensor_values[5] > 80 or sensor_values[6] > 80:  # Right side
            # Turn left  
            left_speed = 3.14
            right_speed = 6.28
        
        # Apply motor speeds
        left_motor.setVelocity(left_speed)
        right_motor.setVelocity(right_speed)
        
        step_count += 1
        
        # Record position (simplified)
        position_history.append({
            'step': step_count,
            'time': step_count * timestep / 1000.0,
            'sensors': sensor_values,
            'motors': [left_speed, right_speed]
        })
    
    print(f"✅ Simulation completed - {step_count} steps")
    return sensor_history, position_history

# Run the simulation
print("🚀 Starting obstacle avoidance simulation...")
sensor_data, position_data = run_obstacle_avoidance()
```

### Step 5: Analyze Results with AI
```python
# Analyze the robot's behavior
import matplotlib.pyplot as plt
import numpy as np

# Extract data for plotting
times = [p['time'] for p in position_data]
front_sensors = [p['sensors'][0] + p['sensors'][7] for p in position_data]  # Combined front sensors
motor_diff = [abs(p['motors'][0] - p['motors'][1]) for p in position_data]  # Turn intensity

# Create visualization
fig, (ax1, ax2) = plt.subplots(2, 1, figsize=(12, 8))

# Plot sensor readings over time
ax1.plot(times, front_sensors, 'b-', linewidth=2, label='Front Obstacle Detection')
ax1.set_xlabel('Time (seconds)')
ax1.set_ylabel('Sensor Value')
ax1.set_title('Robot Obstacle Detection Over Time')
ax1.grid(True)
ax1.legend()

# Plot turning behavior
ax2.plot(times, motor_diff, 'r-', linewidth=2, label='Turning Intensity')
ax2.set_xlabel('Time (seconds)')
ax2.set_ylabel('Motor Speed Difference')
ax2.set_title('Robot Turning Behavior')
ax2.grid(True)
ax2.legend()

plt.tight_layout()
plt.show()

# Calculate performance metrics
avg_obstacle_proximity = np.mean(front_sensors)
total_turning = np.sum(motor_diff)
simulation_time = times[-1]

print(f"📊 Simulation Analysis:")
print(f"   • Total simulation time: {simulation_time:.2f} seconds")
print(f"   • Average obstacle proximity: {avg_obstacle_proximity:.1f}")
print(f"   • Total turning activity: {total_turning:.1f}")
print(f"   • Obstacles encountered: {len([x for x in front_sensors if x > 80])}")
```

Now ask the AI to analyze your results:

> "Can you analyze this robot's obstacle avoidance performance? What improvements would you suggest for the algorithm?"

## Advanced Integration Examples

### 1. Multi-Robot Coordination

```python
# Example: Two robots working together
class MultiRobotController:
    def __init__(self):
        self.robots = []
        self.robot_positions = []
        
    def add_robot(self, robot_name):
        # In Webots, load a world with multiple robots
        robot = Robot()
        self.robots.append(robot)
        print(f"Added robot: {robot_name}")
    
    def coordinate_movement(self):
        # Implement formation keeping or task distribution
        for i, robot in enumerate(self.robots):
            # Assign different behaviors to each robot
            if i == 0:
                self.leader_behavior(robot)
            else:
                self.follower_behavior(robot, i)
    
    def leader_behavior(self, robot):
        # Leader explores the environment
        pass
    
    def follower_behavior(self, robot, robot_id):
        # Followers maintain formation with leader
        pass

# Ask AI to help implement coordination algorithms
multi_robot = MultiRobotController()
```

### 2. Computer Vision Integration

```python
# Robot with camera sensor
def setup_camera_robot():
    robot = Robot()
    camera = robot.getDevice('camera')
    camera.enable(timestep)
    
    return robot, camera

def process_camera_data(camera):
    # Get camera image
    image = camera.getImage()
    width = camera.getWidth()
    height = camera.getHeight()
    
    # Convert to numpy array for processing
    import numpy as np
    
    # Webots image is in BGRA format
    image_array = np.frombuffer(image, np.uint8).reshape((height, width, 4))
    
    # Convert BGRA to RGB
    rgb_image = image_array[:, :, [2, 1, 0]]  # BGR to RGB
    
    return rgb_image

# Use this with AI for object detection:
# "Can you help me detect red objects in this camera image?"
```

### 3. Machine Learning Integration

```python
# Collect training data from simulation
class RobotDataCollector:
    def __init__(self):
        self.training_data = []
    
    def collect_sensor_data(self, robot, action_taken):
        # Get current sensor state
        sensor_values = [sensor.getValue() for sensor in distance_sensors]
        
        # Record state-action pair for learning
        data_point = {
            'sensors': sensor_values,
            'action': action_taken,
            'timestamp': robot.getTime()
        }
        
        self.training_data.append(data_point)
    
    def export_training_data(self):
        import pandas as pd
        df = pd.DataFrame(self.training_data)
        df.to_csv('robot_training_data.csv', index=False)
        print(f"Exported {len(self.training_data)} training samples")

# Use with AI for behavior learning:
# "Can you train a neural network on this robot behavior data?"
```

## Real-World Project Workflows

### Project 1: Autonomous Warehouse Robot

```python
# Step 1: Design the warehouse environment in Webots
# - Create shelving units, boxes, charging stations
# - Add floor markers for navigation

# Step 2: Implement robot controller
class WarehouseRobot:
    def __init__(self):
        self.robot = Robot()
        self.state = "idle"  # idle, navigating, picking, returning
        self.inventory = []
        self.target_location = None
    
    def navigate_to_shelf(self, shelf_id):
        # Use Webots GPS and compass for navigation
        gps = self.robot.getDevice('gps')
        compass = self.robot.getDevice('compass')
        # Implementation details...
    
    def pick_item(self, item_id):
        # Control gripper to pick items
        gripper = self.robot.getDevice('gripper')
        # Implementation details...

# Step 3: Ask AI for optimization
# "How can I optimize the robot's path planning for multiple pickups?"
```

### Project 2: Search and Rescue Robot

```python
# Simulate disaster scenarios
class SearchRescueRobot:
    def __init__(self):
        self.robot = Robot()
        self.victims_found = []
        self.explored_areas = set()
    
    def explore_unknown_terrain(self):
        # Use SLAM algorithms for mapping
        # Ask AI: "Implement a frontier-based exploration algorithm"
        pass
    
    def detect_victims(self):
        # Use camera and thermal sensors
        # Ask AI: "Help me detect human heat signatures in thermal images"
        pass
    
    def plan_rescue_path(self):
        # Optimize path to reach all victims
        # Ask AI: "Find the shortest path visiting all victim locations"
        pass

# Test in various disaster scenarios
rescue_robot = SearchRescueRobot()
```

### Project 3: Agricultural Monitoring Drone

```python
# Simulate crop monitoring with aerial robots
class AgricultureDrone:
    def __init__(self):
        self.robot = Robot()  # Drone in Webots
        self.crop_health_data = []
        self.flight_path = []
    
    def plan_survey_flight(self, field_boundaries):
        # Generate optimal flight path for coverage
        # Ask AI: "Create a lawn mower pattern for field coverage"
        pass
    
    def analyze_crop_health(self, camera_image):
        # Use computer vision for health assessment
        # Ask AI: "Detect diseased plants in this aerial image"
        pass
    
    def generate_health_map(self):
        # Create spatial map of crop conditions
        import matplotlib.pyplot as plt
        # Visualization code...

# Integrate with weather data and growth models
ag_drone = AgricultureDrone()
```

## Advanced Features

### Custom Robot Models

```python
# Load custom robot from URDF or Webots proto files
def load_custom_robot(robot_file_path):
    # In Webots: File → Import → Robot
    # Then connect via Python API
    
    robot = Robot()
    
    # Auto-discover available devices
    devices = []
    device_count = robot.getNumberOfDevices()
    
    for i in range(device_count):
        device = robot.getDeviceByIndex(i)
        device_name = device.getName()
        device_type = device.getNodeType()
        devices.append({'name': device_name, 'type': device_type})
    
    print(f"Found {len(devices)} devices on robot:")
    for device in devices:
        print(f"  • {device['name']} ({device['type']})")
    
    return robot, devices

# Ask AI to help configure your custom robot:
# "Help me set up controllers for my 6-DOF robotic arm"
```

### Physics Parameter Tuning

```python
# Optimize simulation realism
def tune_physics_parameters():
    # Access world physics settings through Webots API
    # Adjust gravity, friction, damping, etc.
    
    physics_configs = [
        {'gravity': -9.81, 'friction': 0.8, 'bounce': 0.2},
        {'gravity': -9.81, 'friction': 1.0, 'bounce': 0.1},
        {'gravity': -9.81, 'friction': 0.6, 'bounce': 0.3},
    ]
    
    results = []
    for config in physics_configs:
        # Run simulation with different physics
        result = run_robot_test(config)
        results.append(result)
    
    # Ask AI to analyze which configuration is most realistic
    return results

# "Which physics parameters give the most realistic robot behavior?"
```

### Performance Monitoring

```python
# Monitor simulation performance and robot metrics
class PerformanceMonitor:
    def __init__(self):
        self.metrics = {
            'simulation_fps': [],
            'robot_speed': [],
            'energy_consumption': [],
            'task_completion_time': [],
            'collision_count': 0
        }
    
    def update_metrics(self, robot):
        # Collect real-time performance data
        current_speed = self.calculate_robot_speed(robot)
        self.metrics['robot_speed'].append(current_speed)
        
        # Monitor energy usage
        motor_power = self.calculate_motor_power(robot)
        self.metrics['energy_consumption'].append(motor_power)
    
    def generate_performance_report(self):
        # Create comprehensive performance analysis
        import pandas as pd
        
        report = pd.DataFrame(self.metrics)
        
        # Ask AI to analyze performance trends
        return report

monitor = PerformanceMonitor()
```

## Troubleshooting Guide

### Common Issues and Solutions

#### Connection Problems
```python
# Test Webots connection
def test_webots_connection():
    try:
        robot = Robot()
        timestep = robot.getBasicTimeStep()
        print(f"✅ Webots connected - timestep: {timestep}ms")
        return True
    except Exception as e:
        print(f"❌ Connection failed: {e}")
        print("Solutions:")
        print("1. Ensure Webots is running")
        print("2. Check Python path includes Webots controller library")
        print("3. Verify world file has robot controller set to '<extern>'")
        return False

test_webots_connection()
```

#### Performance Issues
```python
# Optimize simulation speed
def optimize_simulation():
    # Reduce rendering quality for faster simulation
    # In Webots: View → Rendering → Fast
    
    # Adjust physics timestep
    # Larger timestep = faster simulation, less accuracy
    # Smaller timestep = slower simulation, more accuracy
    
    recommended_settings = {
        'timestep': 32,  # milliseconds
        'rendering': 'fast',
        'run_mode': 'run',  # not real-time
        'physics_iterations': 1
    }
    
    print("Recommended settings for performance:")
    for key, value in recommended_settings.items():
        print(f"  • {key}: {value}")

optimize_simulation()
```

#### Memory Usage
```python
# Monitor and optimize memory usage
import psutil
import os

def monitor_memory():
    process = psutil.Process(os.getpid())
    memory_mb = process.memory_info().rss / 1024 / 1024
    
    print(f"Current memory usage: {memory_mb:.1f} MB")
    
    if memory_mb > 1000:  # More than 1GB
        print("⚠️ High memory usage detected")
        print("Solutions:")
        print("1. Reduce data collection frequency")
        print("2. Clear old simulation data")
        print("3. Restart simulation periodically")
    
    return memory_mb

# Run periodically during long simulations
current_memory = monitor_memory()
```

## Best Practices

### Code Organization
```python
# Structure your robotics projects
"""
robotics_project/
├── robots/          # Robot controller code
├── worlds/          # Webots world files  
├── data/           # Simulation results
├── analysis/       # Data analysis notebooks
└── docs/           # Project documentation
"""

# Use classes for robot controllers
class BaseRobotController:
    def __init__(self, robot_name):
        self.robot = Robot()
        self.name = robot_name
        self.initialize_devices()
    
    def initialize_devices(self):
        # Override in subclasses
        pass
    
    def run_behavior(self):
        # Override in subclasses  
        pass

class NavigationRobot(BaseRobotController):
    def initialize_devices(self):
        # Specific device setup for navigation
        pass
```

### Experimental Design
```python
# Design reproducible experiments
class RoboticsExperiment:
    def __init__(self, experiment_name):
        self.name = experiment_name
        self.parameters = {}
        self.results = []
        self.random_seed = 12345
    
    def set_parameters(self, **params):
        self.parameters.update(params)
    
    def run_trial(self, trial_number):
        # Set random seed for reproducibility
        import random
        random.seed(self.random_seed + trial_number)
        
        # Run simulation trial
        result = self.execute_simulation()
        self.results.append(result)
        
        return result
    
    def analyze_results(self):
        # Statistical analysis of results
        # Ask AI: "Analyze these experimental results for significance"
        pass

# Example usage
exp = RoboticsExperiment("Obstacle Avoidance Performance")
exp.set_parameters(robot_speed=2.0, sensor_range=0.5)
```

### Documentation and Sharing
```markdown
# Always document your robotics projects:

## Project: Autonomous Navigation Robot
**Objective**: Develop obstacle avoidance for mobile robot
**Environment**: Office-like setting with furniture obstacles
**Robot**: Differential drive robot with 8 distance sensors
**Algorithm**: Reactive obstacle avoidance with wall following

### Results Summary
- Successfully navigated 95% of test scenarios
- Average completion time: 45 seconds
- Energy efficiency: 15% better than baseline

### Key Insights from AI Analysis
- Robot tends to favor right turns (bias in algorithm)
- Performance degrades in narrow corridors
- Sensor fusion could improve obstacle detection

### Future Improvements
- Implement path planning for global navigation
- Add camera for better obstacle classification
- Test in dynamic environments with moving obstacles
```

## Resources and Next Steps

### Webots Learning Resources
- [Webots User Guide](https://cyberbotics.com/doc/guide/index)
- [Robot Programming Tutorials](https://cyberbotics.com/doc/guide/tutorials)
- [Sample Robot Projects](https://cyberbotics.com/doc/guide/samples)

### Integration with SnapThink Features
- **[Document Analysis](../features/document-analysis.md)** - Analyze robotics research papers
- **[Python Environment](../features/python-environment.md)** - Advanced data analysis
- **[AI Chat](../features/ai-chat.md)** - Get help with robotics algorithms

### Community and Support
- Join the [Webots Discord](https://discord.gg/nTWbN9m) community
- Share your projects in the [SnapThink Community](https://github.com/snapthink/community)
- Contribute to robotics examples and tutorials

**Ready to build amazing robots?** Start with the quick start example above and ask the AI for help along the way!
