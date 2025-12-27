# Prime-S73P

![Prime-S73P Board](https://github.com/SymTrioS/Prime-S73P/blob/main/Jpg/Prime-S73P_01.jpg)

### A Heterogeneous Computing Platform with Three Integrated Processing Units

---

## Overview

The Prime-S73P is a versatile development board that integrates three distinct computing architectures into a single platform:

- **User Interface Processor** - ARM9-based Linux system for application development and I/O management
- **Hardware Processing Unit** - FPGA for custom logic and hardware acceleration
- **Microcontroller** - ARM Cortex-M4 for real-time analog and digital signal processing

The board includes integrated development tools accessible via a single USB Type-C connection, including a microcontroller programmer/debugger (CMSIS-DAP), dual-channel USB-to-UART converter, and FPGA configuration interface.

---

## Table of Contents

- [Architecture](#architecture)
- [Hardware Specifications](#hardware-specifications)
- [Getting Started](#getting-started)
- [Display Configuration](#display-configuration)
- [Development Resources](#development-resources)
- [Factory Configuration](#factory-configuration)

---

## Architecture

![Functional Diagram](https://github.com/SymTrioS/Prime-S73P/blob/main/Jpg/Prime-S73P_Func.jpg)

### System Components

#### **F1C200S Application Processor**
- ARM9 architecture running at 408 MHz with 64 MB DDR RAM
- Hardware H.264 video decoding (BP/MP/HP profiles) up to 1280×720 @ 30 fps
- NTSC/PAL CVBS video input support
- Peripheral interfaces: USB-A Host, UART, SPI, I²C, GPIO
- Display support: LCD interface (up to 800×480 resolution)
- Camera interface: up to 5 MP sensor
- Operating system: Linux (Debian 12 supported)

#### **Gowin GW1NR-LV9QN88P FPGA**
- Logic resources: 8,640 LUTs, 6,480 flip-flops
- On-chip memory: 468 Kbit RAM, 608 Kbit Flash
- Internal PSRAM: 64 Mbit (4M × 16-bit)
- External Flash: 16 Mbit P25Q16
- Analog front-end: Two 12-bit ADC121 converters
- Wireless connectivity: ESP-03 Wi-Fi module
- Interfaces: HDMI output, GPIO, user-configurable I/O
- Storage: microSD card slot (switchable between FPGA and F1C200S)
- Development environment: Gowin IDE (`.fs` bitstream format)

#### **GD32F303CCT6 Microcontroller**
- ARM Cortex-M4 core at 120 MHz
- Memory: 256 KB Flash, 48 KB SRAM
- External storage: 8 MB SPI Flash (W25Q64), 8 KB I²C EEPROM (M24C64)
- High-precision analog I/O: 16-bit 4-channel ADS1120 ADC, two 12-bit DAC7311 DACs
- Integrated analog peripherals: 2× 12-bit DAC, 2× 12-bit ADC, 3× 12-bit ADC channels
- Communication interfaces: USB, UART, SPI, I²C, CAN, GPIO
- Debug interface: CMSIS-DAP via USB Type-C
- Display support: SPI/GPIO connector for external displays
- Firmware format: Intel HEX (`.hex`)

---

## Hardware Specifications

### F1C200S Microprocessor

| Parameter | Specification |
|-----------|---------------|
| Architecture | ARM9 |
| Clock Frequency | 408 MHz |
| System Memory | 64 MB DDR |
| Video Decoder | H.264 BP/MP/HP up to 1280×720 @ 30 fps |
| Video Input | NTSC/PAL CVBS |
| Host Interface | USB-A |
| Serial Interfaces | UART, SPI, I²C |
| General I/O | GPIO |
| Display Support | LCD interface (up to 800×480 pixels) |
| Camera Support | Up to 5 MP sensor |
| Operating System | Linux (Debian 12) |

### FPGA - Gowin GW1NR-LV9QN88P

| Parameter | Specification |
|-----------|---------------|
| Logic Elements | 8,640 LUTs, 6,480 flip-flops |
| Embedded Memory | 468 Kbit RAM, 608 Kbit Flash |
| Internal Memory | 64 Mbit PSRAM (4M × 16-bit) |
| External Flash | 16 Mbit P25Q16 |
| Analog Inputs | Two 12-bit ADC121 converters |
| Wireless Module | ESP-03 Wi-Fi |
| Video Output | HDMI |
| Storage | microSD card (switchable to F1C200S) |
| Development Tool | Gowin IDE |
| Bitstream Format | `.fs` |

### GD32F303CCT6 Microcontroller

| Parameter | Specification |
|-----------|---------------|
| Core | ARM Cortex-M4 |
| Clock Frequency | 120 MHz |
| Flash Memory | 256 KB |
| SRAM | 48 KB |
| External Flash | 8 MB W25Q64 (SPI) |
| External EEPROM | 8 KB M24C64 (I²C) |
| External ADC | 16-bit 4-channel ADS1120 (SPI) |
| External DAC | Two 12-bit DAC7311 (SPI) |
| Integrated DAC | 2× 12-bit channels |
| Integrated ADC | 2× 12-bit + 3× 12-bit channels |
| Communication | USB, UART, SPI, I²C, CAN |
| General I/O | GPIO, analog I/O |
| Debug Interface | CMSIS-DAP-S via USB Type-C |
| Display Connector | SPI/GPIO |
| Firmware Format | `.hex` |

---

## Getting Started

### Prerequisites

- USB Type-C cable
- Terminal emulator software (e.g., PuTTY, Tera Term, minicom)
- Serial port settings: 115200 baud, 8N1

### Quick Start

1. **Linux image**
   
   The pre-prepared Linux files and instructions for writing them to the uSD card are located in the **uSD** directory.  Instructions for building your own version of the Linux system image are located in the **Doc** directory.

2. **Connect the Board**
   
   Connect the Prime-S73P to your computer using the USB Type-C connector.

3. **Verify Device Enumeration**
   
   Three USB devices should appear in your system:
   
   - **CMSIS-DAP-S** - Microcontroller debugger interface
   - **COM Port 1** - F1C200S UART console
   - **COM Port 2** - GD32F303 UART debug output

   ![COM Port Detection](https://github.com/SymTrioS/Prime-S73P/blob/main/Jpg/Com67.png)

4. **Access the Linux Console**
   
   Open COM Port 1 in your terminal emulator (115200 8N1). When a uSD drive containing a Linux image is inserted, you will see boot messages and a login prompt.
   
   **Default credentials (Debian 12):**
   - Username: `root`
   - Password: `root`
   
   For instructions on building a custom Linux image, refer to the documentation in the `Doc` folder.

5. **Monitor Microcontroller Output**
   
   Open COM Port 2 to view real-time diagnostic output from the GD32F303. The test firmware reports:
   - Cyclic DAC-B voltage values
   - Digitized readings from the ADS1120 ADC
   - Data from the two FPGA-connected ADC121 converters

### LED Indicators

During operation:
- **Blue and Red LEDs** - Blink according to user-defined logic
- **Green LED** - Indicates FPGA register access (external ADC121 readout)

---

## Development Resources

### Microcontroller Firmware

Test firmware is available for two development environments:

- **IAR Embedded Workbench:** [GD32F303CC_IAR](https://github.com/SymTrioS/GD32F303CC_IAR)
- **Visual Studio Code:** [GD32F303CC_VSC](https://github.com/SymTrioS/GD32F303CC_VSC)

![Development Environments](https://github.com/SymTrioS/Prime-S73P/blob/main/Jpg/Prime-S73P_IAR_VSC.png)

### FPGA Development

The FPGA test project for Gowin IDE is available at:

[GW1NR-LV9](https://github.com/SymTrioS/GW1NR-LV9)

![Gowin IDE](https://github.com/SymTrioS/Prime-S73P/blob/main/Jpg/Gowin.png)

### Prebuilt Binaries

Ready-to-use binaries are located in the `Bin` directory:
- **GD32F303 firmware** - `.hex` format
- **FPGA configuration** - `.fs` format

---

## Factory Configuration

The board ships with preloaded test firmware and FPGA configuration. All factory binaries are available in the `Bin` directory for restoration or reference.

---

## Display Configuration

### LCD Panel Connection

The board supports LCD panels with a 40-pin FFC (flat flex cable) connector. Compatible display sizes include:

- 4.3-inch displays (480×272 or 800×480 pixels)
- 5.0-inch displays (800×480 pixels)
- 7.0-inch displays (800×480 pixels)

**Touchscreen support:**
- Resistive touchscreens: Supported
- Capacitive touchscreens: Not supported

![LCD 40-Pin Connector](https://github.com/SymTrioS/Prime-S73P/blob/main/Jpg/LCD_40pin.jpg)

### Resolution Configuration

To configure the display resolution for 480×272 panels:

1. Navigate to the device tree file:
   ```
   linux-5.2-fps/arch/arm/boot/dts/suniv-f1c200s-prime-s.dts
   ```

2. Modify the display timing configuration:
   - Comment out line 31 (800×480 configuration)
   - Uncomment line 29 (480×272 configuration)

3. Rebuild the kernel with the updated device tree

### SPI Display Support

The GD32F303 microcontroller includes a dedicated SPI/GPIO connector for external displays. Example configurations are shown below:

<p align="center">
<img src="https://github.com/SymTrioS/Prime-S73P/blob/main/Jpg/LCD_SPI.png" width="50%">
</p>

---

## Additional Resources

- **Documentation:** See the `Doc` folder for detailed hardware specifications and Linux build instructions
- **Schematics:** Available in the repository for reference
- **Community Support:** Open an issue on GitHub for technical questions or bug reports

---

## License

Please refer to the repository license file for terms of use.

---

**Note:** This board is designed for development and prototyping. Ensure proper thermal management and power supply specifications are met for your specific application requirements.
