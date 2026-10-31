### Bootloader using UART

Aiming to build a bootloader software that would be capable to flash the application software using UART. Checks for valid application before transferring control to application software. Falls back to bootloader if the application is not valid. 

There are still many unknowns on how the project will go forward.

Config:
Board: STM32_Nucleo_F413ZH
Comms: UART3
Indiation: LED Green PB0 - LD1

Flashing steps of bootloader:
- Memory Erase based on the bootloader start address and length of data
- Flash data through UART
- Exit the flash cycle only after fully downloading data
- Reset the software
- Check the bootloader software is loaded

Bootloader application steps:
- check for valid application
- No valid application jump to boot
- bootloader receives programming flag input and enters programming mode
- data downloaded through programming session
- exit the programming session after successful application download
- debounce logic: data transfer failed midway reset to the bootloader again. Discard the data on flash cycle.

Implemenation steps:
- LED test - polling, later to ISR. LED blinks when data received from the UART RX channel.
- UART comms - TX and RX checks.
- memory map to make a distiction between Application flash ranges and Bootloader flash ranges. Reserving area if needed.
