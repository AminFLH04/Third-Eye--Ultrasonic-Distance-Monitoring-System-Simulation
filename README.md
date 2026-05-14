# Third Eye – Ultrasonic Distance Monitoring System (AVR, USART)

The **Third Eye** project is a distance‑measurement and alert system built using two AVR microcontrollers communicating via the **USART serial protocol**.  
The **Master** MCU measures distance using an ultrasonic sensor and sends the measured value to the **Slave** MCU.  
The **Slave** MCU receives the data and controls a **buzzer** (acting as a vibrator) and an **LED**, based on the measured distance. Two buttons allow the user to manually disable the buzzer and LED.

---

## System Overview

The system consists of:

- **Master AVR MCU**  
  Measures distance using an ultrasonic sensor (Trigger/Echo) and transmits the measured distance over USART.

- **Slave AVR MCU**  
  Receives the distance value, decides whether to activate the buzzer/LED, and allows manual shutdown via two external interrupt buttons.

Communication is handled through **USART**, and the system responds automatically to distance changes while providing manual override options.

---

## Master System Operation

The Master microcontroller performs:

### 1. Ultrasonic Distance Measurement
The Master uses a standard ultrasonic sensor with:

- **Trigger pin** – receives a 10 µs pulse from the MCU  
- **Echo pin** – outputs a pulse whose duration corresponds to the distance  

The MCU measures the Echo pulse duration and converts it to distance (in centimeters).

### 2. Sending Data Over USART
After calculating the distance, the Master sends the value as a serial string through USART to the Slave MCU.

---

## Slave System Operation

The Slave microcontroller handles:

### 1. Pin Configuration and Interrupt Setup
- **PB0** → Buzzer output  
- **PB1** → LED output  
- **PC0, PC1** → Buttons connected to **INT0** and **INT1**  

Pressing these buttons triggers external interrupts, turning off the buzzer and LED.

### 2. Receiving Data from Master (USART)
The Slave configures USART to receive data via interrupt:

- Incoming characters are stored in a buffer.
- Data is read until a newline character (`\n`) is received.
- The received string is then parsed into a numeric distance.

### 3. Distance Processing and Output Control
Once the distance value is received:

- If **distance < 70 cm**, the buzzer and LED turn **ON**.
- Otherwise, both remain **OFF**.

Control is handled through simple digital writes to PB0 and PB1.

### 4. Manual Override with Buttons
Using **INT0** and **INT1**, pressing the buttons instantly turns off:

- Buzzer (via INT0)
- LED (via INT1)

This gives the user direct manual control over the alert outputs.

---

## USART Baud Rate Issue and Solution

Originally, the system was configured with:

- **Baud Rate:** 9600  
- **UBRRL:** 6  

However, the communication was unreliable and data was corrupted.

After testing, the baud rate was changed to:

- **Baud Rate:** 4800  
- **UBRRL:** 12  

This adjustment resolved the communication issues and ensured stable data transfer between the Master and Slave MCUs.

This demonstrates the importance of correct USART configuration—especially matching baud rates and tolerances across devices.

---

## Summary

The Third Eye project successfully:

- Measured distance via ultrasonic sensor  
- Sent distance values through USART  
- Received and processed distance on the Slave MCU  
- Controlled buzzer/LED based on distance threshold  
- Allowed manual shutdown with external interrupts  
- Resolved serial communication problems by optimizing the baud rate  

This project highlights the importance of proper USART configuration and the usefulness of interrupt‑based control in embedded systems.


# DOWNLOAD

To get the project files, please email afallah009@gmail.com.
