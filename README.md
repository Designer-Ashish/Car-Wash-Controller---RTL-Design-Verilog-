# **Car Wash Controller - RTL Design (Verilog)**

![Car Wash System](docs/block_diagram.png)  
*A finite state machine (FSM) based car wash controller implemented in Verilog HDL.*

## **📌 Table of Contents**
- [Features](#-features)
- [Block Diagram](#-block-diagram)
- [Design Details](#-design-details)
- [Simulation](#-simulation)
- [Synthesis](#-synthesis)
- [Future Improvements](#-future-improvements)
- [License](#-license)

---

## **✨ Features**
- **FSM-based control** with 6 states:  
  `IDLE → SOAP → WATER → BRUSH → DRY → DONE`  
- **Timed operations** (adjustable wash durations)  
- **Coin-operated start** (requires `coin_inserted` signal)  
- **Emergency stop** functionality (`stop` resets to `IDLE`)  
- **Status LEDs** for user feedback (`00=Idle`, `01=Washing`, `10=Done`)  

---

## **📊 Block Diagram**
```
                    +-------------------+
                    |   User Inputs     |
                    | (Start, Stop,    |
                    |  Coin Sensor)     |
                    +---------+---------+
                              |
                    +---------v---------+
                    |   Control Logic   |
                    |    (FSM)          |
                    +---------+---------+
                              |
                    +---------v---------+
                    |   Timer/Counters  |
                    +---------+---------+
                              |
                    +---------v---------+
                    |   Output Control  |
                    | (Pumps, Motors,   |
                    |  Dryer, LEDs)    |
                    +-------------------+
```

---

## **🔧 Design Details**
### **📜 States & Transitions**
| State | Description | Next State Condition |
|-------|-------------|----------------------|
| `IDLE` | Waits for coin & start | → `SOAP` if `coin_inserted & start` |
| `SOAP` | Activates soap valve | → `WATER` after `SOAP_TIME` |
| `WATER` | Sprays water | → `BRUSH` after `WATER_TIME` |
| `BRUSH` | Runs brush motor | → `DRY` after `BRUSH_TIME` |
| `DRY` | Activates air dryer | → `DONE` after `DRY_TIME` |
| `DONE` | Wash complete | → `IDLE` |

### **⏱ Timing Parameters**
- `SOAP_TIME` = 0.5 sec  
- `WATER_TIME` = 1 sec  
- `BRUSH_TIME` = 1.5 sec  
- `DRY_TIME` = 2 sec  

*(Adjustable in RTL code)*

---

## **🎛 Simulation**
### **1. Install Tools**
```bash
# Linux (Debian/Ubuntu)
sudo apt install iverilog gtkwave

# Windows (Winget)
winget install IcarusVerilog.IcarusVerilog
winget install GTKWave.GTKWave
```

### **2. Run Simulation**
```bash
iverilog -o sim src/car_wash_tb.v src/car_wash_controller.v
vvp sim
gtkwave car_wash.vcd
```

### **📊 Expected Waveforms**
| Signal | Description |
|--------|-------------|
| `leds` | `00`=Idle, `01`=Washing, `10`=Done |
| `soap_valve` | High during `SOAP` state |
| `water_valve` | High during `WATER` state |
| `brush_motor` | High during `BRUSH` state |
| `dryer` | High during `DRY` state |

---

## **⚙ Synthesis**
### **FPGA Implementation Steps**
1. Add `.v` files to your FPGA tool (Vivado/Quartus)  
2. Set clock constraints (e.g., 50MHz)  
3. Map I/O:  
   - `clk` → FPGA clock pin  
   - `reset_n` → Push button  
   - `start/stop/coin_inserted` → Switches  
   - `leds` → GPIO LEDs  

---

## **🚀 Future Improvements**
- [ ] **PWM control** for motor speed adjustment  
- [ ] **Sensor integration** (water level, brush pressure)  
- [ ] **Multi-cycle wash modes** (Quick/Premium wash)  
- [ ] **7-segment display** for countdown timer  
- [ ] **UART interface** for remote control  

---

## **📜 License**
This project is licensed under **MIT License**.  
See [LICENSE](LICENSE) for details.

---

## **📂 Repository Structure**
```
/car_wash_controller  
│── /src  
│   ├── car_wash_controller.v   # Main RTL  
│   └── car_wash_tb.v           # Testbench  
│── /docs  
│   └── block_diagram.png       # System overview  
│── README.md                   # This file  
└── run_sim.sh                  # Auto-run simulation  
```

---

**🎉 Happy Washing!** 🚗💦
