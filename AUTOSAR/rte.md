# RTE (Runtime Enviroment):
- Implements the VFB on each ECU.
- Its the only interface between SW-C and BSW Modules.
  

## VFB(Virtual Functional Bus):
- High level communication abstraction.
- During design pahse, the SW-C are partitioned to ECUs
  - Called SW-C-to-ECU mapping.
- This result in two diferent types of communication paths:
  - Intra-ECU.
  - Inter-ECU.