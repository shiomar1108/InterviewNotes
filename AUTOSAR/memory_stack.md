# Memory Stack Interview Notes:
- Service Layer:
  - NVRam Manager.
- ECU Abstraction Layer:
  - Memory Abstraction Interface.
  - Flash-EEPROM Emulator.
  - EEPROM Abstraccion.
- Microcontroller Abstraction Layer:
  - Flash Driver.
  - EEPROM Driver.


## Memory Blocks:
- Can have:
  - Header (Optional).
  - Data.
  - CRC (Optional).
- Three type of blocks:
  - Native Block.
  - Redudant Block.
  - Data Set Blocks.


## Fee:
- Flash-EEPRON Emulation.
- Organized in Pages.
- Grouped in banks/sectors (Erasable Unit).
- Clear entire section.
- Defaults is "1s".
- Writting is add "0s".
- Slow.
- Writting invalidates previous content, until page is full, then copy valid information to new page and clear all the page.
  - Keep working in newer page.

## EA:
- EEPROM Abstraction.
- Seemingly unlimited number of Write Cycles per block.