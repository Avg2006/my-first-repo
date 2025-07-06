# 🦾 Arm Drive ROS Node

This is a ROS1 Python node designed to control a robotic arm using joystick input (`sensor_msgs/Joy`). It reads axes and button states, processes them into meaningful motion commands, and publishes a 6-element array over the `stm_write` topic to be used by a microcontroller or hardware interface.

---

## 📦 Topic Communication

### **Subscribed Topic**
- `/joy_arm` – Type: `sensor_msgs/Joy`  
  → Input coming from the joystick/gamepad.

### **Published Topic**
- `/stm_write` – Type: `std_msgs/Int32MultiArray`  
  → Output array with six control values to be sent to the STM32 (or any controller).

---

## 🎮 Joystick Mapping Logic

Here's what the joystick inputs are mapped to:

| **Index** | **Signal** | **Mapped From**         | **Note**                                                |
|-----------|------------|--------------------------|----------------------------------------------------------|
| `0`       | Shoulder   | `-axes[1]`               | Vertical joystick for up/down movement                   |
| `1`       | Base       | `axes[0]`                | Horizontal joystick for base rotation                    |
| `2`       | Roll/Pitch | `buttons[0] + buttons[1]`| Controlled using combination of X, A, etc.               |
| `3`       | Elbow      | `axes[3]`                | Another axis for joint control                           |
| `4`       | Gripper    | `-axes[2]`               | Trigger for gripping motion                              |
| `5`       | Roll/Pitch | `buttons[1] - buttons[0]`| Used with index 2 to define pitch/roll intent & direction|

---

## ⚙️ How It Works

- Initializes a ROS node called `arm_drive`
- Listens to joystick input (`/joy_arm`)
- Converts joystick axes/buttons to control signals
- Packs the signals into a 6-element `Int32MultiArray`
- Publishes at **50 Hz** to `stm_write`

Each loop cycle creates a message using the `createMsg()` method which takes a shallow copy of the output buffer to avoid side effects during publishing.

---

## 🛠️ To Run the Node

```bash
rosrun <your_package_name> arm_drive.py
