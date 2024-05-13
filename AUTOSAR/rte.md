# RTE (Runtime Enviroment):
- Implements the VFB on each ECU.
- Its the only interface between SW-C and BSW Modules.
  - Communication infraestructure.
- Use ports.
- Type of Communication:
  - Inter-ECU.
  - Intra-ECU.
  - Inter-Core.
  - Inter-Partitions.
  - Intra-Partition.
- Responsible of scheduling runnables.
- Check for message consistency.
- Communication between SW-C.

## VFB(Virtual Functional Bus):
- High level communication abstraction.
- During design pahse, the SW-C are partitioned to ECUs
  - Called SW-C-to-ECU mapping.
- This result in two diferent types of communication paths:
  - Intra-ECU.
  - Inter-ECU.
- Flexible mapping of SW-C.
- Standarize RTE.


## RTE Layer Scheduling:
- Event-to-task mapping.
  - `Rte_Task_<TaskName>(fg)`
- Can include conditions to avoid runnable triggers.


## RTE Generator:
- Generator base on configuration
- Input files:
  - SWC Description.
  - ECU Extract Description.
  - EcuC values, Os, RTE, Com.
  - ECU Instances.
  - BswM Descriptions.
- Output Files:
  - RTE Layer.
    - RTE.c / RTE.h / RTE_Lib.c / etc.
  - Application specific headers.
    - `RTE_<SW-CNamr>.h`
  - Further configuration files:
    - Os related ARXML Cfg.
    - IOC releated ARXML Cfg.
    - Support data ARXML Cfg.
    - BswM-D.arxml.


## Partitions:
- RTE supports mapping of Software components to different partitions of an application.
- A partition is represented by an Os application.
- For each Os application, a separate RTE is generated.
- A partition must be allocated to exactly one core, but  multiple partition can be allocated on the same core.
### Comunication between partitions:
- IOC
- SMC
- Mix between them.
#### IOC (Inter Os application Communicator):
- Consist on channels and channels groups.
#### Shared Memory Communicator (SMC):
- Based on share memory concepts.
- RTE implements the functionality.
- Spinlocks or exclusive areas are used to prevent multiple access.
### Inter partition Comm C/S:
- Higher runtime that intrapartition direct C/S comm.
- Influence in Task Mapping.
- Mapping is requiered if `Allow Inter Partition Direc Call` is false.
- Client must be mapped to extended task.


## RTE Interfaces:
### Sender Reciever:
- `Rte_Write_<PortName>_<VariableDataPrototype>(<value>)`.
- `Rte_Read_<PortName>_<VariableDataPrototype>(&<value>)`.
### Client Server:
- `Rte_CAL_<PortName>_<Operation>(<value>, <argument2>, <argument3>)`.