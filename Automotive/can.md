# CAN Interview Notes:
- Control Area Network.
- Serial Bus protocol.
  - Use MSB going first.
- Asynchronous.
- Multicast. type (like Broadcast).
- Multi-master.
- Small frame size:
  - 8 bytes.
  - Speed: 1Mbps at 40 meters.
- Industry ussually use 500 kbps:
  - 1 second means 2000 CAN frames.
- Frame:
  - PCI.
  - Data.
- Base on voltage difference:
  - Logic 1: Recesive.
  - Logic 0: Dominant.
|    | CAN H | CAN H | delta V |
|----|-------|-------|---------|
| L0 | 2.5   | 2.5   | 0       |
| L1 | 3.5   | 1.5   | 2       |
- CAN Transciever convert TTL logic to CAN logic.
- Half-duplex communication.
  - For some node is either Tx or Rx.


## CAN Messages:
- Message base protocol.
- Use Message ID:
  - Identify Message.
  - Establish message priority.
  - Used in message filters.
- ACK field is in the same frame.
  - No more bus load.
- CSMA-CA: Carrier Sense Multiple Access - Collision Avoidance.
- Temporary Node Failure:
  - Ongoing frame destroyed.
- Permanent Node Failure:
  - Node no longer connected to be bus.
  - No Tx nor Rx.
### Frame Types:
#### Data Frame:
- Standard Frame:
  - 11 bit message ID.
- Extended Frame:
  - 29 bit message ID.
    - 11 bit base + 18 bit ID extention.
#### Remote Frame:
- Frame to request a Data Frame.
- A node that wants a Data Frama send the remote frame with same Msg ID.
- The node that owns the data frame send it immediatly after the remote frame.
- Obsolete.
#### Error Frame:
- Signal a error condition in the data frame being transmitted.
- If a node detect an error, it destroys the message and signals all nodes by Tx an error frame.
#### Overload Frame:
- Send by a node when it's overloaded and needs some time to process thed ata frame received.
- Format is the same as error frame.
- The idea is buy time by keeping the bus busy with overload frame preventing new data frame bein Tx.
- Max 3 overload frame can be Tx by each node after a data frame.


## Standard Data Frame Format:
- Bus Idle:
  - Recesive State (Logic 1).
- Start of frame:
  - SOF.
  - Dominant bit (Logic 0).
- MSG Id:
  - 11bit (0x000 - 0x7FF).
- RTR (Remote Transmit Request):
  - 1 bit
    - Dominant bit (Logic 0) -> Data Frame.
    - Recesive bit (Logic 1) -> Remote Frame.
- R1/R0 :
  - Recessive (2 bits).
  - Reserved.
    - R1 -> IDE: used to differenciate Standard and Extended.
    - R0 -> FDF: used to differenciate CAN and CanFD.
- DLC (Data Length Code):
  - 4 bit (0 to 8).
- Data:
  - 0 to 8 bytes.
- Checksum:
  - 15 bit.
  - BCH method.
- CRC Delimet:
  - CD.
  - 1 bit.
  - Recessive (Logic 1).
- ACK:
  - 1 bit.
  - Tx node put a recessive bit in ACK.
  - All Rx nodes who wants to acknowledge will put a Dominant in ACK.
- ACK Delimet:
  - AD.
  - 1 bit.
  - Recessive (Logic 1).
- EOF (End of Frame):
  - Recessive (Logic 1).
  - 7 bits.
- IFS (Inter Frame Spacing):
  - Recessive (Logic 1).
  - 3 bits.
### Frame sections:
- Arbitration Field:
  - SOF + Msg Id + RTR.
- Control Filed:
  - R0 + R1 + DLC.
- Data Field:
  - Data.
- CRC Field:
  - Checksum + CD.
- Acknowledgement Field:
  - ACK + AD.
- End of Frame Field:
  - EOF + IFS.


## Extended Frame Format:
- 11bit message allow for 512 frames per entwork, in real application it's not enough.
- Extended frame uses 29bit Msg ID, 18 more called IDE.
- Standard frame and Extended frame CAN Frames can co-exist.
- Arbitation Filed:
  - SOF (1bit) + Msg ID (11bit) + SRR (1bit) + R1(IDE)(1bit) + Extended ID (18bits) + RTR.
  - SRR -> Substitute for Remote Request.
### Additional bits:
- SRR:
  - Substitute Remote Request.
  - Always Recessive (Logic 1) and single bit.
- IDE:
  - Identifier Extension Indication bit.
  - Indicate presence of Extended Identifier.
    - Recessive (Logic 1) -> Extended.
    - Dominant (Logic 0) -> Standard.


## CAN Bit Segmentation:
- Bit monitoring -> monitoring the bus for bit value.
  - Tx node puts the bit value on bus.
  - Rx node sample the bus ans reads the bit value.
### CAN bit segment:
- Bit Segments: Point of time in bit value does sampling.
- Sync_Seg.
- Prop_Seg.
- Phase_Seg1.
  - Bit sample point.
- Phase seg2.
### Propagation Delay:
- Propagation segments compensate for propagation delay on the bus.
- Tx of a bit and propagation must be over for all nodes on the bus before the bit sampling has to happen to ensure all the nodes read the same value for a bit on the bus.


## CAN Bus Arbitration:
- Bus arbitration -> resolution to conflict for multiple Tx nodes transmiting simultaneously.
- SOF + Msg ID + RTR are involved.
- The node that win will Tx the complete frame and the other will became Rx nodes.
- Lossing modes will Tx after the bus goes IDLE.
- Only one transmitter is allowed to Tx at the time.
- Other node wait for Bus IDLE.
- Iterate over every bit and a Dominant bit win (Logic 0) so the other became Rx.
- Lowest Msg ID -> Higher priority.


## Bit Stuffing: