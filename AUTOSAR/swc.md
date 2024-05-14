# Software Component (SW-C) Interview Notes:
- Two parts:
  - SW-C Description:
    - .ARXML
  - SW-C Source code:
    - *.c / *.h
- Follow AUTOSAR Specification:
  - `AUTOSAR_TPS_SoftwareComponentTemplate.pdf`

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
### Provider:
- Sent information to a SW-C.
### Required:
- Reciev infomration from a SW-C.
### Provider/Required:
- Has the capability of both send and recieve information.
### Sender-Reciever (S/R):
- Asynchronous.
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
- Hold the functionality of the software.
### Sensor-Actuator:
- Make the functionality of a sensor or an actuator usable for other SW-C.
- Linked to a specific Hardware.
- Cannot be relocated.
### Parameter:
- Provide calibration only data to the software.
### Composition:
- Atomic Software components together to adress a complete functional implementation of the software.
- Agrregates components and connection between it sub-components.
### Service Proxy:
- Acts as a proxy to provide access to internal services.
- 1:M
- Used when a particular service component is to be accessed from a different control unit.
### Service:
- Interact directly with Bsw Modules.
- Used for configuring services for a particual control unit.
### ECU Abstraction:
- Part of Bsw.
- I/O Hardware abstraction.
- Provide access to ECUs specific I/O Capabilities.
- Act as an interface between MCAL and Sensor Actuator Components on the Application SW-C.
### Complex Device Driver (CDD):
- Model a function outside normal Bsw.
- For HW not directly supported by AUTOSAR.
- Directly access to HW.
- Provide easy access to hardware directly from application later to fulfill special timming.
### Nv Block:
- Provide access to non-volatile data.
- Allow modeling of NV data at VFB Level.
- Used when having interfaces on the Aplication layer to be sotred on NV Memory.


## Runnable:
- Part of SW-C.
- Smallest Unit. / Smallest code fragment that is provided by the SW-C.
- Schedule by Os.
- Need an entry point for each runnable.
- Only part of Atomic SW-C (Not Composition).
- `<symbol>` define the name of the C function.
- Excecuted and Scheduled independtly.
- Started by RTE.
- Implemented as C functions.
- Must be mapped to a Task, as taks is the owner of resources and priority.
### Events, Timmers, Interrupts:
- Periodic events that can trigger a Runnable:
  - Timmer.
  - Schedule table.
  - Interrupt.
  - Data Receiving.
    - Sender reciever Comm - Inter ECU.
      - `Com_SendSignal`.
      - `Com_ReceiveSignal`.
### Explicit:
- Data is send as is.
- RTE done't check message consistency.
### Implicit:
- RTE will check for message consistency.


## Connectors:
- Used to complete the connection between prototypes.
- Assembly connector:
  - Provider-Required.
  - Inside Composition.
- Delegation Port:
  - Same por type.
  - Different Composition.
- Pass through Connector.


## Events:
- It specifies the Os on when and how call runnables.
- Timming is based on seconds.
### General Events:
- Init event.
- Timming event.
- External trigger occurred event.
- Internal trigger occurred event.
- Background event.
### Client Server Events:
- Operation Invoke Events.
- Asynchronous Server call result event.
### Data Events:
- Data Write complete event.
- Data Send complete event.
- Data Receive event.
- Data Recieve Error event.
### Mode Events:
- Mode Switch event.
- Mode Manager Error event.
- Mode Switch ACK event.


## ARXML File:
- Software Component Type: Aplication SW-C.
```
├─ Ports
│  ├─ P-Ports
│  │  ├─ S/R Interface
│  │  └─ C/S Interface
│  └─ R-Ports
│     ├─ S/R Interface
│     └─ C/S Interface
├─ Internal Behaviour
│  ├─ Events
│  │  ├─ Timming Event
|  |  ├─ XYZ Evenet
│  │  └─ Init Event
│  └─ Runnables
│     ├─ Function_1
│     │  └─ Data Access
│     └─ Function_XYZ
│        └─ Data Access
└─ SWC Implementation
   └─ Delivery Info
```


## C File:
- Add RTE Application header:
  - `#include "Rte_<SWCName>.h`.
  - One configured runnable for each function in hte C file.