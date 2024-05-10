# AUTOSAR Diagnostic Stack Notes:
- OBD.
- UDS.

## Modules:
### DEM (Diagnostic Event Manager):
- Processing diagnostic evenets.
- Storing events and event data to NVM.
- Provide information to DCM.
- Define DTCs.
### DCM (Diagnostic Communication Manager):
- Communication with external tools.
- DTC or DID.
- Recieve request -> Validate service -> Take ACTION -> Reply.
- ISO 14229.
### Path:
::: mermaid
graph TD;
    Application <--> RTE
    RTE <--> DCM
    DCM <--> DEM
    DCM <--> PduR
    PduR <--> BusTp
    BusTp <--> BusIf
    BusIf <--> BusDriver
:::
#### DCM Layers:
- DSL (Diagnostic Session Layer):
  - Accept request from PduR and pass it to DSD. Other way as well.
  - Check for:
    - Tester present.
    - Security Level of ECU.
    - Keep track of current session.
    - Protocol Timming.
    - Handling between OBD and UDS.
- DSD (Diagnostic Service Dispatcher):
  - Validate requested Service and Send request to DSP.
  - Check for security and Session access of the incomming request.
  - Check for service support.
  - Collect response from DSP, and built message to DSL.
- DSP (Diagnostic Service Processor):
  - Perform check and excecutes particulat action based on request.
  - Check for message format.
  - Assemble part of the response.
  - Service implementation.

### FIM (Function Inhibition Manager):
- Inhibition of particular functionalities of software components based on evenet status.
- Uses FID (Function Identifier).
- FID is assigned to SWC.
- Based on event estatus, FID's status is derived and whcih will decide whether to execute the functionality or not.
### DET (Development Error Trace):
- Only for development.
- Provide APIs to report an error.
- Each error has a number.
- Each module define their information about DET.