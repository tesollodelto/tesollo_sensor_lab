# Tesollo Sensor Lab

![License](https://img.shields.io/badge/License-BSD_3--Clause-blue.svg?style=flat-square)
![Python](https://img.shields.io/badge/Python-3.11-3776AB?style=flat-square&logo=python)
![OS](https://img.shields.io/badge/OS-Ubuntu_22.04-E95420?style=flat-square&logo=ubuntu)
![Isaac Lab](https://img.shields.io/badge/NVIDIA-Isaac_Lab-76B900?style=flat-square&logo=nvidia)

Welcome to the **Tesollo Sensor Lab** repository. 
This is the repository providing Tesollo finger tip sensor configurations, data structures, and compiled modules.

## Overview
This repository contains the compiled Python modules (`.so` files) required to interface with Tesollo sensors in simulation environments, explicitly designed for **NVIDIA Isaac Lab**.

These core modules handle:
* **`tactile_sensor.so`**: Core functions for tactile sensor operations and data streaming.
* **`tactile_sensor_cfg.so`**: Sensor configuration classes.
* **`tactile_sensor_data.so`**: Data structures for processing raw tactile feedback.

> **Important Compatibility Note:**
> The provided `.so` files are pre-compiled binaries. 
> Ensure your environment matches the following to avoid `ImportError` or `ModuleNotFoundError`:
> * **Operating System:** Ubuntu 22.04 (Linux x86_64)
> * **Python Version:** Python 3.11 (with Isaac Sim/Lab)

## Installation

Since these are pre-compiled modules, you do not need to build them from source. Simply clone this repository and include the `tesollo_sensors` directory in your Python/Isaac Lab project.

```bash
git clone https://github.com/tesollodelto/tesollo_sensor_lab.git
```

Move the `tesollo_sensors` folder into your working directory, or ensure it is added to your `PYTHONPATH`.

## Usage in Isaac Lab

Here is an example of how to configure and inject the Tesollo sensor into your Isaac Lab `InteractiveSceneCfg`.

**1. Define the Sensor Configuration**
Import the `TesolloSensorCfg` from the compiled module and define the properties:

```python
from tesollo_sensors.tactile_sensor_cfg import TesolloSensorCfg

# Configure the Tesollo Sensor
DELTO_TACTILE_SENSOR_CFG = TesolloSensorCfg(
    prim_path="{ENV_REGEX_NS}/robot/.*_tactile/tactile_sensor",
    contact_object_prim_path_expr="{ENV_REGEX_NS}/object",
)
"""Configuration of Tesollo Tactile Sensor."""
```

**2. Inject into the Scene Configuration**
Use the `.replace()` method to attach the sensor configuration to specific links on your robot within the scene configuration class:

```python
import isaaclab.sim as sim_utils
from isaaclab.scene import InteractiveSceneCfg
from isaaclab.utils import configclass

@configclass
class SceneCfg(InteractiveSceneCfg):
    ground = AssetBaseCfg(prim_path="/World/defaultGroundPlane", spawn=sim_utils.GroundPlaneCfg())
    
    # Replace with your actual robot configuration
    robot = DELTO_HAND_TACTILE_CFG.replace(prim_path="{ENV_REGEX_NS}/robot")  
    
    # Attach Tesollo tactile sensors to specific links
    tactile_sensor_1 = DELTO_TACTILE_SENSOR_CFG.replace(
        prim_path="{ENV_REGEX_NS}/robot/link_1_tactile/tactile_sensor"
    )
    tactile_sensor_2 = DELTO_TACTILE_SENSOR_CFG.replace(
        prim_path="{ENV_REGEX_NS}/robot/link_2_tactile/tactile_sensor"
    )
    # ... add more sensors as needed
```

## Repository Structure

```text
tesollo_sensor_lab/
├── tesollo_sensors/
│   ├── __init__.py
│   ├── tactile_sensor.so
│   ├── tactile_sensor_cfg.so
│   └── tactile_sensor_data.so
├── LICENSE
└── README.md
```

## Contact & Support

For technical support, inquiries about sensor hardware, or collaboration opportunities:

* **Website:** https://www.tesollo.com/
* **Email:** support@tesollo.com
