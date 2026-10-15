# Nyquix

**A mixed-signal hardware platform and real-time digital signal processing (DSP) engine powered by STM32.**

## Overview

**Nyquix** is a dedicated hardware and firmware platform designed for real-time digital signal processing applications, including digital filtering (FIR/IIR). Built around an ARM Cortex-M core with hardware floating-point and DSP instructions.

## Key Features

- **Deterministic Real-Time DSP Pipeline:** Zero-CPU-overhead sample acquisition using timer-triggered ADC/DAC conversions and circular DMA ping-pong buffering.  
- **Hardware-Accelerated Math:** Utilizes ARM CMSIS-DSP and internal hardware accelerators (FPU / CORDIC / FMAC) for single-cycle Multiply-Accumulate (MAC) operations.  
- **Low-Noise Mixed-Signal Front-End:** Active anti-aliasing filter (AAF) and reconstruction filter stages with isolated analog/digital power domains.  
- **MATLAB-to-Target Workflow:** Seamless coefficient export and hardware-in-the-loop (HIL) verification scripts.

## Specifications

*Spec. below are DRAFT*

| Parameter | Specification |
| :---- | :---- |
| **MCU Core** | STM32 (ARM Cortex-M4 / M7 with FPU) |
| **Sampling Rate (\$f\_s\$)** | Up to 100 kSPS (configurable via Timer trigger) |
| **ADC / DAC Resolution** | 12-bit / 16-bit (Hardware / External Codec) |
| **Analog Input Bandwidth** | DC to 45 kHz (-3 dB Anti-Aliasing Cutoff) |
| **Analog Input Range** | \$\\pm 1.5\\text{ V}\$ differential / \$0\\text{–}3.3\\text{ V}\$ single-ended |
| **Supply Voltage** | 5V USB-C input with ultra-low-noise LDO regulation |

*Specifications describe design targets unless explicitly identified as measured or verified results.*

## Getting Started

### Prerequisites

- **Hardware:** Nyquix board, ST-LINK debugger, Signal Generator, Oscilloscope.  
- **Tools:** KiCad (v10.0+), STM32CubeMX, STM32CubeIDE.

### Flashing the Firmware

1. Clone the repository to your local workspace 
2. Open the firmware/ directory in **STM32CubeIDE**.  
3. Build the Release configuration (ensuring \-O3 optimization and hardware FPU flags are enabled).  
4. Connect the ST-LINK to the SWD header and flash the target.

## Architecture

### Components

- **Analog Front-End (AFE):** Handles input impedance matching, DC bias level shifting (\$V\_{ref}/2\$), and active Sallen-Key / MFB anti-aliasing filtering prior to digitization.  
- **Acquisition Engine (DMA \+ Timers):** A hardware timer triggers ADC conversions at a strict sampling period (\$T\_s\$), transferring samples into a dual-buffer array via DMA (Half-Complete and Transfer-Complete interrupts).  
- **DSP Core:** Processes buffered blocks using biquad cascade IIR or direct-form FIR structures, outputting results to the DAC buffer before the next DMA swap.

### Design Considerations

- **Mixed-Signal:** Continuous solid ground plane with strict physical partitioning between high-speed digital switching lines (SWD, SPI/I2S, MCU clock) and sensitive analog traces to minimize crosstalk and noise floor elevation.  
- **Analog Power Integrity:** Dedicated LC \$\\pi\$-filter and high-PSRR LDO feeding VDDA and operational amplifiers to prevent digital rail ripple from coupling into the ADC reference.  
- **Execution Latency vs. Block Size:** DMA ping-pong buffer sizes are tuned to balance interrupt overhead against input-to-output phase delay (group delay).

## Repository Structure
```
Nyquix/
├── README.md
├── hardware/              
│   ├── Nyquix.kicad\_pro
│   ├── Nyquix.kicad\_sch
│   └── Nyquix.kicad\_pcb
├── firmware/
│        
```

## Documentation

| Resource | Description |
|---|---|
| [Getting Started](path/to/file) | Usage, setup, or contribution guidance |
| [Architecture](path/to/file) | Architecture or design documentation |
| [Validation](path/to/file) | Test, review, or verification evidence |
| [Other](path/to/file) | Additional documentation |

## Results

[Optional: Summarize verified test results, measurements, evaluations, or review outcomes. Omit this section if not applicable.]

## References

[Optional: List relevant standards, references, dependencies, or related projects.]

## License

This project is licensed under the MIT License \- see the [LICENSE](http://LICENSE) file for details.
