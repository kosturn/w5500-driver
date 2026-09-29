# W5500 TCP/UDP driver

Minimal WIZnet ioLibrary for IPv4 TCP/UDP communication with a PC.

## Build

Compile `socket.c`, `wizchip_conf.c`, and `w5500.c`.
Add this `w5500-driver` directory to the compiler include paths.
The corresponding headers are retained. `_WIZCHIP_` defaults to `W5500`;
other chips are not supported by this trimmed repository.

## MCU integration

1. Initialize MCU SPI/GPIO peripherals and reset the W5500 as required by the board.
2. Register SPI and chip-select callbacks with `reg_wizchip_spi_cbfunc()` and
   `reg_wizchip_cs_cbfunc()`. Register critical-section callbacks with
   `reg_wizchip_cris_cbfunc()` when concurrent access is possible.
3. Initialize socket buffers with `wizchip_init()` and check its return value.
4. Configure a static MAC address, IPv4 address, subnet mask, and gateway using
   `wizchip_setnetinfo()`. Use a unique MAC/IP and configure the PC on the same
   subnet for a direct connection. DHCP and DNS clients are not included.
5. Use the socket API from `socket.h`:
   - TCP server: `socket(..., Sn_MR_TCP, ...)`, `listen()`, `getSn_SR()`,
     `send()`, `recv()`, `disconnect()`, `close()`.
   - TCP client: `socket(..., Sn_MR_TCP, ...)`, `connect()`, `send()`,
     `recv()`, `disconnect()`, `close()`.
   - UDP: `socket(..., Sn_MR_UDP, ...)`, `sendto()`, `recvfrom()`, `close()`.

MCU hardware callbacks and the PC application are supplied by the consuming
project. Examples, Internet protocol modules, other chip drivers, and compiled
help files have been removed. Original copyright notices and `license.txt`
are retained.
