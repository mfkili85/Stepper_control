# Stepper_control

A FreeRTOS-based embedded application for the **Tiva C Series LaunchPad (TM4C123GH6PM)** that controls a unipolar stepper motor using a **ULN2803 Darlington Transistor Array**. 

This project demonstrates real-time task scheduling, inter-task communication using FreeRTOS queues, and precise low-side driver sequencing to control motor rotations based on button inputs.

---

## Features
* **Modular Codebase**: Separates hardware-specific stepper control and FreeRTOS task logic into dedicated driver files (`Stepper_task.c` and `Stepper_task.h`).
* **FreeRTOS Integration**: Uses an asynchronous, non-blocking task for motor driving, allowing the CPU to execute other tasks simultaneously.
* **Queue-Driven Commands**: Communicates button press events to the motor task using a thread-safe FreeRTOS queue.
* **300-Step Rotations**: Commands the motor to rotate exactly 300 steps clockwise (Right Button) or 300 steps counter-clockwise (Left Button).
* **Power-Saving Coils Release**: Automatically de-energizes the stepper motor coils after completing a movement to prevent the motor and the ULN2803 from overheating.
* **Precise 10ms Step Timing**: Employs RTOS-friendly delays to achieve smooth 10ms step transitions without blocking the processor.

---

## Hardware Configuration

### Pin Mapping

| Tiva C Pin | ULN2803 Input Pin | ULN2803 Output Pin | Motor Coil / Wire | Connection Details |
| :--- | :--- | :--- | :--- | :--- |
| **PD0** | Pin 1 (1B) | Pin 18 (1C) | Coil A (e.g., Blue) | Control Signal |
| **PD1** | Pin 2 (2B) | Pin 17 (2C) | Coil B (e.g., Pink) | Control Signal |
| **PD2** | Pin 3 (3B) | Pin 16 (3C) | Coil C (e.g., Yellow) | Control Signal |
| **PD3** | Pin 4 (4B) | Pin 15 (4C) | Coil D (e.g., Orange) | Control Signal |
| **GND** | Pin 9 (GND) | — | — | Common Ground |
| — | Pin 10 (COM) | — | Common (e.g., Red) | Connect to External VCC |

#### Button Inputs (Example Pinout)
* **PF4 (Left Button / SW1)**: Sends a counter-clockwise command (300 steps).
* **PF0 (Right Button / SW2)**: Sends a clockwise command (300 steps).

> [!WARNING]  
> **Inductive Kickback Protection**: You **must** connect Pin 10 (COM) of the ULN2803 to your motor's positive power supply (VCC). This activates the internal freewheeling clamp diodes. Omitting this connection will cause high-voltage inductive kickback spikes to destroy the ULN2803 and potentially damage your Tiva C board. Always power the motor using an external power supply (e.g., 5V or 12V), and link its ground to the Tiva C's GND.

---

## Software Architecture

The system is split into two asynchronous execution paths communicating via a FreeRTOS Queue:

## Demo

Below is a demonstration of the system in action. When a button is pressed, the event is queued, and the motor executes a precise 300-step rotation before releasing the coils.

### Visual Demonstration
<video src="https://github.com/user-attachments/assets/f93ac3cb-3900-4f80-818f-365924ce830a" width="100%" controls>
</video>
