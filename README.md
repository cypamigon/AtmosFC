# AtmosFC

<p align="center"> <img src="documentation/logo/atmos-fc_logo_black-bg.png" alt="AtmosFC Logo" /> </p> 

AtmosFC is an open-hardware quadcopter flight controller designed in KiCad. Powered by an STM32G4 MCU, it integrates high-performance sensors and peripherals, and is fully compatible with Betaflight (≥ 4.5).

<p align="center"> <img src="documentation/atmos-fc_images/pcb_1.png" width="500" alt="AtmosFC PCB"/> </p> 

## Table of Contents
- [Features](#features)
- [Input / Output](#input--output)
- [Hardware Architecture](#hardware-architecture)
- [Flashing Betaflight](#flashing-betaflight)
- [Notes](#notes)
- [Gallery](#gallery)
- [Repository Structure](#repository-structure)
- [License](#license)

## Features

- **MCU**: STM32G473CEU6
- **IMU**: ICM-468-P
- **Barometer**: DPS-386
- **OSD**: MAX7456 for analog on-screen display
- **Blackbox**: 32MB onboard flash memory for log recording
- **Power input**: Up to 8S
    > ⚠️   The board has been tested with 6S LiPo batteries. The onboard MAX25232 buck regulator can theoretically support up to 36 V (~ 8S LiPo), but this has not been tested. Use higher voltages at your own risk.
- **Power monitoring**: Battery voltage and current sensing
- **Status indicators**: Green, Blue, and Amber LEDs
- **Dimensions**: 40x40mm
- **Mounting holes**: 30.5×30.5 mm (M3)
- **Weight**: 8.2g

## Input / Output 

- **USB connector**: USB Type-C
- **Motor outputs**: x4
- **ESC**: Current sensing and telemetry
- **UARTs**: x3
- **I2C**: Yes
- **Video Input / Video Output with OSD** 
- **5V outputs**: up to 3A total 
- **Buzzer output**

## Hardware Architecture

The block diagram below shows the main hardware components and their interconnections.

<p align="center"> <img src="documentation/atmos-fc_block_diagram.png" alt="AtmosFC Hardware Block Diagram"/> </p> 

##  Flashing Betaflight

AtmosFC runs **Betaflight ≥ 4.5**. You can either flash the precompiled Betaflight 4.5 firmware or build your own from source.

### Option 1 – Using the provided firmware
1. Download `betaflight_config/betaflight_4.5.0_ATMOSFC.hex`.
2. Open **Betaflight Configurator**.  
   If you don’t have it installed, download the latest version here : [Betaflight Configurator Releases](https://github.com/betaflight/betaflight-configurator/releases/latest)
3. Connect AtmosFC via USB-C.
4. Enter DFU mode if required (hold BOOT button while connecting USB).
5. In Configurator, go to Firmware Flasher → select the `.hex` file.
6. Flash and reboot.

### Option 2 – Building from Source

> **Note:** Building Betaflight requires some development tools (make, GCC, etc.).  
> See the official instructions here for setup on your operating system:: [Betaflight Building Docs](https://betaflight.com/docs/category/building).

1. Clone the [Betaflight repository](https://github.com/betaflight/betaflight).
2. Add the AtmosFC board configuration by copying the folder `betaflight_config/ATMOSFC/` into `betaflight/src/config/configs`.
3. Update the target list by running:
```
> make configs
```
4. Then you can make a build for the AtmosFC target.
```
> make ATMOSFC
```
5. Flash the compiled `.hex` using Betaflight Configurator (refer to steps from option 1).

## Notes 
- The board has been **successfully tested with Betaflight 4.5**. All basic functions, including motor outputs, sensors, LEDs, were working correctly.
- **I did not test the OSD feature**, as I was flying using digital FPV goggles instead of analog video.
- There is a small **voltage drop on the VBAT measurement** (~0.2-0.3V) due to the Schottky diode used for reverse polarity protection.  
  - This can be compensated either by applying a software offset in Betaflight, or by using an alternative reverse polarity protection method, such as a MOSFET-based circuit

## Gallery

<p align="center">
  <img src="documentation/atmos-fc_images/pcb_2.png" width="400" />
    <br>
  <img src="documentation/atmos-fc_images/pcb_3.png" width="400"  />
    <br>
  <img src="documentation/atmos-fc_images/pcb_4.png" width="400"/>
    <br>
  <img src="documentation/atmos-fc_images/pcb_3d.png" width="400"/>
      <br>
  <img src="documentation/atmos-fc_images/pcb_integrated.png" width="400"/>
</p>

## Repository Structure

The repository is organized as follows:

``` bash
├── hardware/             
│   ├── atmos_fc/           # KiCad project files (.kicad_pcb, .sch, etc.)  
│   ├── assembly/           # Fabrication files (Gerber, BOM)
│   └── lib/                # Custom footprints, symbols, 3D models
├── betaflight_config/
│   ├── ATMOSFC/            # Board configuration for Betaflight ≥ 4.5
│   └── betaflight_4.5.0_ATMOSFC.hex    # Precompiled Betaflight 4.5 firmware
├── documentation/          # Logo, block diagram, photos
└── README.md
```

##  License

This project is released under the **[MIT License](LICENSE.txt)**.  
You are free to use, modify, and distribute it.
