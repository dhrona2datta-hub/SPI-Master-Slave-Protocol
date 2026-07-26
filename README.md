# SPI-Master-Slave-Protocol
A Verilog-based SPI (Serial Peripheral Interface) Master with three SPI Slave devices, designed and simulated in Xilinx Vivado. The project demonstrates synchronous full-duplex communication, multi-slave selection using dedicated Chip Select (CS) lines, and FPGA-ready digital design principles.
Overview

The SPI Master with Three SPI Slaves project is a digital communication system designed using Verilog HDL that demonstrates the working of the Serial Peripheral Interface (SPI) protocol. The design consists of one SPI Master and three SPI Slave devices connected through common communication lines: MOSI (Master Out Slave In), MISO (Master In Slave Out), and SCLK (Serial Clock). Each slave has an individual Chip Select (CS) signal, allowing the master to communicate with one slave at a time. The project is simulated in Xilinx Vivado and is suitable for FPGA implementation, making it an excellent example of synchronous serial communication.

Working Principle

The communication begins when the master receives a start signal, an 8-bit input data, and a slave selection signal. Based on the value of the slave_select input, the master activates one of the three Chip Select lines (CS0, CS1, or CS2), enabling only the chosen slave while the remaining slaves remain inactive. The master then generates the SPI clock (SCLK), which synchronizes the communication between the master and the selected slave.

During each clock cycle, the master transmits one bit of data through the MOSI line while simultaneously receiving one bit of data from the selected slave through the MISO line. This demonstrates the full-duplex nature of the SPI protocol, where data transmission and reception occur simultaneously. After all eight bits are transferred, the master deactivates the Chip Select signal, stops the clock, stores the received data in the output register, and asserts the done signal to indicate that the communication has been completed successfully.

Master Module

The SPI Master is the central controller of the communication system. It is responsible for generating the serial clock, selecting the appropriate slave device, transmitting serial data through MOSI, receiving data through MISO, and controlling the overall communication sequence using a Finite State Machine (FSM). The FSM consists of three states: IDLE, where the master waits for the start signal; TRANSFER, where serial communication takes place; and FINISH, where the transfer is completed and the system returns to the IDLE state. This structured design makes the master reliable, modular, and easy to extend.

Slave Module

A single generic SPI Slave module is used and instantiated three times to represent the three slave devices. Each slave continuously monitors its Chip Select signal and becomes active only when its corresponding CS line is asserted low by the master. While selected, the slave receives the master's data through the MOSI line and simultaneously transmits its own predefined 8-bit data through the MISO line. In this project, Slave 1 transmits 0x3C, Slave 2 transmits 0xA7, and Slave 3 transmits 0xF5. This parameterized approach improves code reusability and follows standard FPGA design practices.

Top Module

The spi_top module integrates the entire system by instantiating one SPI Master and three SPI Slave modules. It connects the common SPI communication signals and uses a MISO multiplexer to ensure that only the selected slave's output is connected back to the master's MISO input. The top module serves as the complete hardware design and simplifies the overall project hierarchy by bringing all modules together into a single integrated system.

Testbench

The testbench verifies the functionality of the complete SPI communication system by generating the system clock, applying reset, selecting different slaves, and providing different input data values. During simulation, the testbench performs three separate communication cycles: first with Slave 1, then Slave 2, and finally Slave 3. After each transfer, the received data is displayed in the Vivado console, confirming that the master correctly communicates with the selected slave while ignoring the inactive slaves. This ensures that the SPI controller operates correctly under different communication scenarios.

Simulation Results

The simulation demonstrates successful SPI communication between the master and each slave. During the first transfer, the master sends 0xA5 to Slave 1 and receives 0x3C. During the second transfer, the master sends 0x55 to Slave 2 and receives 0xA7. Finally, during the third transfer, the master sends 0xF0 to Slave 3 and receives 0xF5. The waveform clearly shows the operation of the MOSI, MISO, SCLK, Chip Select, and done signals, confirming correct full-duplex communication and proper slave selection.

Conclusion

This project successfully demonstrates the implementation of a multi-slave SPI communication system using Verilog HDL. It highlights key digital design concepts such as synchronous serial communication, master-slave architecture, finite state machine control, and full-duplex data transfer. The modular structure, reusable slave design, and successful Vivado simulation make this project suitable for FPGA implementation, academic learning, and portfolio presentation. It also provides a strong foundation for future enhancements such as support for multiple SPI modes, variable data widths, clock division, and error detection mechanisms.
