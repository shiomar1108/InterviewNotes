# Communication Buses Notes:


## UART:
- Universal Asynchronous Reciever Trnasmitter.
- 2 Lines:
  - TxD -> Data Transmition.
  - RxD -> Data Reception.
- Asynchronous communication.
- Speed: from 230 kbps to 460 kbps.
- Frame:
  - Start bit.
  - 8bit data.
  - Stop bit.
- Point to point communication.
  - RS232 / RS485.
- Can have parity.
- Full duplex.
### Software Configuration:
1. Configure registers for Rx and Tx modes and Parity.
2. Configure Baudrate.
3. Configure Status register and Direction.
4. Tx and wait for a bit to send.


## SPI:
- Serial Peripheral Interface.
- Master - Slave principle.
- Full duples (TX/RX at the same time).
- No ACK but a handshake.
- Multiple Slave.
- Simple SW but Harder HW.
- Speed -> Up to 10Mbps to 20 Mbps.
- Synchronous Communication.
- Short distance (Same board).
- Uses less energy.
- No Arbitration.
- 4 lines:
  - Sreal Clock.
  - Slave Select.
  - Master Output - Salve Input (MOSI).
  - Master Input - Slave Output (MISO).
- Frame:
  - 8  bits of information unit.
  - Slave select.
### Operation modes:
1.
2.
3.
4.
### Software Configuration:
1. Define ports with the register of the MCU.
2. Configue ports by setting the registers. 
3. Configure Tx and Rx Functions.
4. Configure Interrupt with flags for Tx and Rx.


## I2C:
- Serial Communication.
- Multi-Master / Multi-Slave.
- Bus type.
- 2 lines:
  - SDA -> For Data.
  - SCL -> For Clock.
- Open drain.
- Semiduplex (one direction).
- Speed between 100 kbps up to 3.4 Mbps.
- Distance up to 32 cm.
- Need a Pull-up resistance (to Vcc).
- Synchronous communication.
- Opne drain signal (Up time in the hundred of nanoseconds ).
- Frame:
  - Start bit.
  - End Bit.
  - ACK bit every 8 bits.
  - ` 1 start bit + 7 bit address + 1bit (read/write) + 1 bit ACK + 8 bit data + 1 bit ACK + 8 bit Data + 1bit ACK + 1 stop bit `
### Hardware Setup:
- Vcc.
- GND.
- SDA.
- SCL
- Pull up resistor.
- Serial resistor.
- Wire capacitor.
- Cross channel capacitor.
### Arbitration:
- If SDA is low (when it should be high) stops transmittion.
### Software Configuration:
1. Define ports with the register of the MCU.
2. Configue ports by setting the registers. 
3. Create Start / Stop / RX and TX functions.
4. Use it in the code (Start -> TX/RX -> Stop).
```
Logic behind TX:
Cycle it 8 times:
    if(data & 0x80)
        SDA = 1
    else
        SDA = 0
    DATA << 1
+ Manual handling of the Clock (SCL)
```


## MQTT:
- Message Protocol.
- Publish/Subscribe.
- Topic Subscription.
- Asychronous.
- Secure Sockets Layer -> Identity / Authentication / Autorization.
- TCP/IP -> Brokes like: Mosquitto / RabbitMQ.
```
MQTT Client ->Publish -> MQTT Broker <- Subscribe <- MQTT Client (Mobile Phone / Backend System)
```
### Quality of Services Level:
- QoS0: At most once.
- QoS1: At least once.
- QoS2: Exactly once.