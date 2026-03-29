# Smart 2D LiDAR Radar & Active Security System

## 📖 Short Description of Functionality
This project is a 2D scanning system (Mini LiDAR Radar) that maps its surrounding environment in real-time and acts as an active proximity warning system. Built around the STM32 Nucleo-U545RE-Q microcontroller, the system mechanically sweeps a Time-of-Flight (ToF) laser sensor to detect obstacles. The resulting 2D map is rendered on an OLED display. Additionally, it features an active security layer: a PWM-driven buzzer and directional LEDs that alert users when objects breach a predefined critical distance.

## ⚙️ Requirements

### Hardware Requirements
* **MCU:** STM32 Nucleo-U545RE-Q Development Board
* **Sensor:** VL53L0X / VL53L1X Time-of-Flight (ToF) Laser Sensor (I2C)
* **Actuator:** 28BYJ-48 Stepper Motor with ULN2003 Driver
* **Display:** 0.96" SSD1306 OLED Display (I2C)
* **Alert System:** 1x Passive Buzzer, 3x LEDs (Red, Yellow, Green), 3x 220Ω Resistors
* **Mechanics:** 3D Printed Enclosure and Rotary Mount

### Software Requirements
* **Language:** Rust (`no_std` / bare-metal)
* **Framework:** Embassy (Asynchronous execution)
* **Toolchain:** `thumbv8m.main-none-eabihf` target
* **Flashing/Debugging:** `probe-rs`
* **Key Crates:** `embassy-stm32`, `embassy-executor`, `defmt`, `ssd1306`, `embedded-graphics`, `micromath`

## 🏗️ Hardware and Software Design

### Hardware Design
The STM32U5 acts as the central controller. The stepper motor sweeps the VL53L0X sensor across a 180-degree field of view. The MCU reads distance data via I2C and translates polar coordinates into Cartesian coordinates. This data is pushed via the same I2C bus to the OLED display. The GPIO and Timer peripherals drive the status LEDs and the PWM passive buzzer based on proximity thresholds.

> [!NOTE] 
> Add your hardware schematic or wiring diagram here.
> `![Hardware Schematic](./images/schematic.png)`

### Software Design
The firmware is entirely asynchronous, utilizing the Embassy framework to ensure non-blocking execution. 

**Core Async Tasks:**
1. **Motor Control Task:** Steps the motor at precise intervals using `embassy-time`.
2. **Sensor Task:** Polls the VL53L0X sensor via I2C, calculates the $X, Y$ coordinates, and pushes the data to a synchronization channel.
3. **UI Task:** Pulls data from the channel and uses `embedded-graphics` to draw the radar interface on the OLED display.
4. **Security Task:** Evaluates the distance; if an object is within the critical zone, it triggers the PWM buzzer and the corresponding directional LED.

> [!NOTE] 
> Add your software flowchart or state machine diagram here.
> `![Software Architecture](./images/software_arch.png)`

## 🚀 How to Run

1. Clone the repository:
   ```bash
   git clone [https://github.com/](https://github.com/)[YourUsername]/stm32-lidar-radar.git
   cd stm32-lidar-radar
