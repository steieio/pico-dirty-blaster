# JBC to UF2 Utility

This tool converts an Altera JBC file into a UF2 file that can be loaded into a microcontroller to program an FPGA.  It takes an existing JBC player firmware file in UF2 format and adds the JBC data along with a header describing the JBC file to the player firmware.

## Generating a JBC File

JBC is the STAPL JAM Byte Code format.  Quartus can generate these files.  In order to fit within the memory constraints of a microcontroller, compression is not supported.  Instructions for generating an uncompressed JBC file can be [found here](https://www.intel.com/content/www/us/en/support/programmable/articles/000079036.html).  

To build get an uncompressed .jbc file:

 1.  Go to Device \> Device and Pin Options \> Programming Files and select the "JEDEC STAPL Format File (.jam)" checkbox, or add ```set_global_assignment -name GENERATE_JAM_FILE ON``` to your Quartus project settings
 2.  Run ```quartus_jbcc.exe -n <filename>.jam <filename>.jbc```

## Example

```python3 uf2jbc.py -u pico-jbi.uf2 -j blinky.jbc -b 0x10020000 -a "PROGRAM" -d "MAX10 Blinky" -o max10_blinky.uf2```

## Arguments

 * -u, --uf2 \<firmware.uf2\>    JBC player firmware file in UF2 format
 * -j, --jbc \<jbc_file.uf2\>    JBC binary file of FPGA image
 * -b, --base \<JUF2 header address\>    Address of the JUF2 header
 * -a, --action \<Action String\>    String indcating action to perform \(Usually "PROGRAM")
 * -d, --description \<Description String\>    String describing the JBC image
 * -s, --speed \<TCK speed in Hz\>    TCK signal speed in Hertz \(to be implemented\)
 * -o, --output \<output.uf2\>    Output filename

## JBC Player Implementations

### pico-jbi.uf2

This is a JBC player implementation for the RP2040 based Raspberry Pi Pico board

 * JUF2 Header Address
   * 0x10020000
 * Pin Assignment
   * TCK:  18
   * TMS:  19
   * TDI:  16
   * TDO:  17

