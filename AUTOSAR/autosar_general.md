# General AUTOSAR Notes:
- `AUTomotive Open System ARchiteture`.
- Created o 2003.
- Based on 3 layers:
  - Application Layer.
  - Runtime Enviroment.
    - Virtual Functional Bus.
      - Abstraccion set of RTE.
      - Handle communication within the ECU and between ECUs.
  - Basic Software:
    - Standarized Sofware Layer.
    - Divided in thre levels.
      - `Services`.
      - `ECU Abstraction`.
      - `MCU Abstraccion`.
- Metamodel -> UML 2.0.
- Specification only.


## Methodology:
### System Configuration Description:
### ECU Extract:
### ECU Configuration Description:

## MCAL:
- Software module that has direct access to all the on-chip MCU Periferals.

## AUTOSAR Classic:
- Based on OSEK Os:
  - Real time control system.
  - Safety Critical.
  - Real time requirements.
- Uses Embedded C.

## AUTOSAR Adaptive:
- Based on Posix OS:
  - Common set of APIs for UNIX-like operating systems.
  - Medium to high real time application.
- Uses C++.
- High performance computing Hardware.
- Secure on-board communication.
- Service orientes Architecture.
- Ethernet with SOME/IP.