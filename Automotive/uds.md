# UDS Protocol:
- Standard Protocol for Testing Communication.
- Collection of Diagnostic Services:
  - Requesting for Data.
  - Writting some Data.
  - Running Test / Getting results back.
  - Flashing a program.
  - Cleaning the memory.
- Service Request:
  - Client-Server Topology.

## UDS Messages:
- Each service has a Service Identifier
  - SID = 1 Byte.
    - 0x00 - 0x3E.
- Subfunction:
  - Optional.
- Data Identifier:
  - DID
  - 2 byte length.
  - Identifier for an Specific Data Element.
    - OBD -> DID's are standarized globally.
    - UDS -> OEM Define their own DIDs.
  - Can be called RID for Routine services or UID or MID (Optional).
- Data Record:
  - Optional.
  - n bytes.
  - New value to be used by the Service.


## Positive Response Message:
- SID (1 byte) + 0x40.
- Sub-function Byte:
  - Optional.
  - Same as request.
- DID Bytes:
  - Same as request.
- Data Record:
  - Depends on each service.


## Negative Response:
- NR_SID (1 byte) = 0x7F.
- SID_RQ (1 byte) -> Same as the requested service.
- NRC (1 byte) -> Reason for the server not able to perform the service.
  - List defined by UDS.