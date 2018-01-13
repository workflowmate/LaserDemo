# LaserDemo
Generate test patterns for laser galvanometer

Based on "Arduino Laser Show With Real Galvos" by DeltaFlo:

http://www.instructables.com/id/Arduino-Laser-Show-With-Real-Galvos/


## Target Device

  **Adafruit HUZZAH ESP8266**
  
Memory usage:
```
Sketch uses 483399 bytes (46%) of program storage space. Maximum is 1044464 bytes.
Global variables use 41296 bytes (50%) of dynamic memory, leaving 40624 bytes for local variables. Maximum is 81920 bytes.
```

## User Interface

  The web page allows you to select objects for display:

![Screenshot](Images/Screenshot.png)

- The Object and Generator lists are generated dynamically from the contents of the objName and genName arrays.
- **KPPS** is the speed of the scanner, Kilo Positions Per Second
- **LTD** Laser Toggle Delay, the latency of the scanner, in milliseconds
- **LQ** Laser Quality, maximum line length

## Adding Objects

### Convert an ILDA file to an include object (.h)

Convert the ILD file to text using LaserBoy http://laserboy.org/code/LaserBoy_2017_08_06.zip:

- i input
- 1 ILD
- filename
- 1 replace
- o output
- 4 text
- 3 all frames
- filename

Convert the text file to an include file (.h):

```
scripts/convert.pl filename
```

### Add initializer code to the bottom of the include file.  For example:

```
...
0x49d0b65,
0x49d0b65,
};


void ilda12k() {
  objectCount++;  
  objectAddress[objectCount] = draw_ilda12k;
  objectName[objectCount] = "ILDA12K";
  objectSize[objectCount] = sizeof(draw_ilda12k)/sizeof(uint32_t);
}
```

### Include the file in the program:
```
#include "ilda12k.h"
```

### Initialize the object in the setup() section:

```
  ilda12k();

```
