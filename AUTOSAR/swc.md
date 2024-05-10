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
### Sender-Reciever (S/R):
- S/R Unqueued:
  - Write (Send).
  - Read (Received).
- S/R Queued:
  - Need to be in a loop.
  - Send (Send).
  - Receive (Received).
### Client-Server (C/S):
- Operation
- n:1 (Multiple Clients one single server).
- `Rte_call_<port>_<operation>(args)`.
### Trigger Ports:
- Trigger the excecution of a Runnable.
### Parameter:
- Use to load constants.
### Mode Switch:
- Uses Mode Declaration Group.
- Enumate States.
### NVData:
- Non-volatile Block Component Type.
- Offers Data-based-access.
- Can be use instead of Memory Services