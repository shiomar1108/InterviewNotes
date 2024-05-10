# Software Component (SW-C):
- Two parts:
  - SW-C Description:
    - .ARXML
  - SW-C Source code:
    - *.c / *.h

## Process:
1. Create SW-C Descriptor:
   - AUTOSAR Authoring tool.
2. Generate SW-C Header files:
   - RTE Generator.
   - Contract Phase.
3. Provide the C Source code implementation:
   - Source code editor.
4. Compile the complete SW-C:

## Ports:
- Hardware independant.
- Standarized.
- Interoperability of Application SW-C.
### Sender-Reciever (S/R):
- Exchange data.
- 1 Sender : N receivers.
- N senders : 1 reciever.
- Error handling.
#### Implicit S/R.
  - Receiving runnable will always read the same data until runnable returns.
  - Data element is copied only when the runnable start.
  - S/R Unqueued:
    - Write (Send):
      - Data is send when the runnable exit.
      - `Rte_Iwrite_<Runnable>_<Port>_<DataElement>`.
    - Read (Received):
      - Data is read when the runnable starts.
      - `Rte_IRead_<Runnable>_<Port>_<DataElement>`.
#### Explicit S/R:
  - Runnable will always read the latest data.
  - S/R Queue:
    - Send (Send):
      - `Rte_Send_<Port>_<DataElement>`.
    - Recieve (Received).
      - `Rte_Receive_<Port>_<DataElement>`.
  - S/R Unqueue:
    - Write (Send):
      - `Rte_Write_<Port>_<DataElement>`.
    - Read (Received).
      - `Rte_Read_<Port>_<DataElement>`.
### Client-Server (C/S):
- Synchronous operation.
- Direct call:
  - Need to maarked as Invoke concurrently, so you don't need to map it to any task.
- Make it re-entrant.
- Map it to a high priority task.
- n:1 (Multiple Clients one single server).
- `Rte_call_<port>_<operation>(args)`.
#### Base case for Server calls:
- Task Context Switch.
- Block until finished.
- Incomming request are queued.
### Trigger Ports:
- Trigger the excecution of a Runnable.
- No data exchange needed.
### Parameter:
- Use to load constants.
- Use to load calibration data.
### Mode Switch:
- Notifies a SW-C pf a mode change.
- Uses Mode Declaration Group.
- Enumate States.
- One 1:n.
### NVData:
- Non-volatile Block Component Type.
- Need a default value.
- Can be owned by a NVBlock
- Offers Data-based-access.
- Can be use instead of Memory Services
### Delegation Ports:
- Ports of composition components.
- Compisite components cannot have service ports.


## Per-Instance Memory:
- Used to store global variables.
- RTE allocates memory statically for each SW-C.


## Application SW-C Types:
### Application SW-C:
- Atomic block.
- HW Independent.
- Movable accros MCUs.
- Has communication with Bsw using AUTOSAR Services.
- Realize a defined functionality on application level.
### Sensor-Actuator:
- Make the functionality of a sensor or an actuator usable for other SW-C.
- Linked to a specific Hardware.
- Cannot be relocated.
### Parameter:
### Composition:
### Service Proxy:
- Acts as a proxy to provide access to internal services.
- 1:M
### Service:
- Interact directly with Bsw Modules.
### ECU Abstraction:
- I/O Hardware abstraction.
- Provide access to ECUs specific I/O Capabilities.
### Complex Device Driver (CDD):
- Model a function outside normal Bsw.
- For HW not directly supported by AUTOSAR.
- Directly access to HW.
### Nv Block:
- Provide access to non-volatile data.
- Allow modeling of NV data at VFB Level.