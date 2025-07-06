# Brevel_Arm_Read_ME

This is a python script designed to control a robotic arm using joystick input. It reads axes and button, processes them into commands, and publishes a 6column - 1row array over a topic.

---

## Topic Communication

### **Subscribed Topic**
- `/joy_arm` –Msg Type: `sensor_msgs/Joy`  
=> Input coming from the joystick.

### **Published Topic**
- `/stm_write` – Type: `std_msgs/Int32MultiArray`  
=> Output array with six values to be sent.

---

## Joystick Mapping Logic

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

## How It Works

1) Initializes a ROS node called `arm_drive`
2) Listens to joystick input (`/joy_arm`)
3) Converts joystick axes/buttons to control signals
4) Packs the signals into a 6-element `Int32MultiArray`
5) Publishes at **50 Hz** to `stm_write`

Each loop cycle creates a message using the `createMsg()` method which takes a shallow copy of the output buffer to avoid side effects during publishing.
