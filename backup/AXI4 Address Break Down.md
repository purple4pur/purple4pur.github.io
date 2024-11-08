### Must know

- Unit of address is **Byte**.
    - 0x00 maps to a 1-byte space in memory.
    - 0x01 maps to the next byte.
- **Aligned address** means `Address % Size == 0`.
    - where `Size` is num of bytes per transfer: `2 ** AxSIZE` or `1 << AxSIZE`.
- **Narrow transfer** means a transfer not using all WDATA, and selected by WSTRB.
- WSTRB should match AWSIZE.

### Schematic diagram

What the memory looks like:

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

### Burst INCR address calculation

> AXI Spec $$A4.1.6

```systemverilog
function AXI_Address calc_Nth_start_addr(AXI_Address start_addr, int axlen, int axsize);
    // TODO
endfunction
```

```systemverilog
function AXI_Address calc_end_addr(AXI_Address start_addr, int axlen, int axsize);
    int total_len = axlen + 1;
    int size = 2 ** axsize; // num of bytes per transfer
    AXI_Address aligned_addr = start_addr / size * size; // round down to aligned address
    AXI_Address end_addr = aligned_addr + total_len * size - 1;
    return end_addr;
endfunction
```