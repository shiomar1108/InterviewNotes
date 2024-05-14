# Mode Management Interview Notes:
- ECU State (Bsw Mode) Manager.
- Watchdog Management.
- Communication Managment.
- Can be used to track system state changes.
- Defining Modes is a special case of S/R communication.
- One SW-C act as as Mode Manager (Mode Machine Instance).
- Mode users will listen to mode changes.
- Mode Disabling Dependencies are used to disable runnables during certain modes.
- Mode Switch events are trigger:
  - On Enter.
  - On Exit.


## StartUp Process:
- EcuM (ECU State Manager):
  - On -> Start up One (Start Os) -> Start Up Two (Init SchM. Init BswM)
    - SchM : Basic Software Scheduler Module.
    - BswM: Basic Software Mode Manager
- BswM:
  - Start up Two A -> Start up Two B -> Run Two.


## Shutdown Process:
- BswM:
  - Prep to Shutdown -> Go off One A -> Go off One B.
- EcuM:
  - Go off Twoo -> Off.


## Sleep Process:
- BswM:
  - Prep Shutdown -> Go Sleep.
- EcuM:
  - Go Sleep -> Sleep (Loop until wake up).


## Wake Up:
- EcuM:
  - Wake up / Re-start -> Wake up validation -> Wake up reaction.
- BswM:
  - Wake up two -> Run Two.


## BswM Rules:
- Rules -> Action True.
- Rules -> Action False.


## Watchdog Stack:
```mermaid
graph LR;
    WdgM --> WdgIf
    WdgIf --> WdgExt
    WdgIf --> Wdg
    Wdg --> ro(On chip Watchdog)
    WdgExt --> SPI
    SPI -->sq[Off chip Watchdog]
```


## ComM (Communication Manager):
- Handle communication modes for each channel.
- Handle bus error managment.
- Support Partial Networks.
- Offers an API to siable sending of signals to prevent the ECU from waking the bus.