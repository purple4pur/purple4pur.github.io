### Must know

- Unit of address is **Byte**.
    - 0x00 maps to a 1-byte space in memory.
    - 0x01 maps to the next byte.
- **Aligned address** means `Address % Size == 0`.
    - where `Size` is num of bytes per transfer: `2 ** AxSIZE` or `1 << AxSIZE`.
- **Narrow transfer** means a transfer not using all *WDATA* bytes, but selected by *WSTRB*.
- *WSTRB* valid bits should match `Size`.

### Schematic diagram

What an AXI address in memory looks like:

- xDATA width: 32 bits

```
       Bit# |31    24|23    16|15     8|7      0|
            +--------+--------+--------+--------+
AXI Address |  0x03  |  0x02  |  0x01  |  0x00  |
            +--------+--------+--------+--------+
            |  0x07  |  0x06  |  0x05  |  0x04  |
            +--------+--------+--------+--------+
            |                              ...  |
            +-----------------------------------+
```

### Calculation of burst INCR addresses

> Reference: AXI Spec $$A4.1.6

A burst INCR request can start from an **aligned addr** or **unaligned addr**. For both cases, the 2nd address is always the next aligned address. Each transfer will step forward 1 `Size` space, making the whole burst INCR request writing to or reading from a continuous memory space.

- Example function to get the start address of the Nth transfer (Nth starts from 1):

```systemverilog
function AXI_Address calc_Nth_addr(AXI_Address start_addr, int axsize, int Nth);
    int size = 2 ** axsize; // num of bytes per transfer
    AXI_Address aligned_addr = start_addr / size * size; // round down to aligned address
    // Nth starts from 1
    AXI_Address Nth_addr = (Nth == 1) ? (start_addr)                      :
                                        (aligned_addr + (Nth - 1) * size) ;
    return Nth_addr;
endfunction
```

- Example function to get the last address of the whole burst INCR request:

```systemverilog
function AXI_Address calc_end_addr(AXI_Address start_addr, int axsize, int axlen);
    int total_len = axlen + 1;
    int size = 2 ** axsize; // num of bytes per transfer
    AXI_Address aligned_addr = start_addr / size * size; // round down to aligned address
    AXI_Address end_addr = aligned_addr + total_len * size - 1;
    return end_addr;
endfunction
```