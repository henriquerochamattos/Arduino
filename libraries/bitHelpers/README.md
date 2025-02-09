
[![Arduino CI](https://github.com/RobTillaart/bitHelpers/workflows/Arduino%20CI/badge.svg)](https://github.com/marketplace/actions/arduino_ci)
[![License: MIT](https://img.shields.io/badge/license-MIT-green.svg)](https://github.com/RobTillaart/bitHelpers/blob/master/LICENSE)
[![GitHub release](https://img.shields.io/github/release/RobTillaart/bitHelpers.svg?maxAge=3600)](https://github.com/RobTillaart/bitHelpers/releases)


# bitHelpers

Arduino library with functions on bit level

## Description

This library contains functions to manipulate bits and bit patterns in an 
efficient way. 
For most functions a 8 - 64 bit optimized version exist. 

The library is tested on ESP32 and Arduino UNO but not for all possible values. 
other platforms are expected to work without modification. 
If they don't please let me know.

New bit functions can be added or investigated, please post an issue.


## Interface


### 0.1.0

BitCount, several implementations to compare performance.
- **uint8_t bitCountReference(uint32_t val)** returns nr of bits set in a value.
- **uint8_t bitCountKR(uint32_t val)** Kerningham Ritchie bitCount
- **uint8_t bitCountArray(uint32_t val)** count per nybble with lookup table
- **uint8_t bitCountF1(uint32_t val)** SWAG algorithm variant
- **uint8_t bitCountF2(uint32_t val)** SWAG algorithm variant

BitCount - fastest version, SWAG algorithm
- **uint8_t  bitCount(uint8_t val)** available for 16, 32 and 64 bit.

Reverse: uint8_t .. uint64_t
- **T bitReverse(T val)** reverses bits in a uint8_t .. uint64_t
- **T nybbleReverse(T val)** reverses nibbles (4 bit) in a uint8_t .. uint64_t
- **T byteReverse(T val)** reverses bytes (8 bit) in a uint16_t .. uint64_t
- **T wordReverse(T val)** reverses words (16 bit) in uint32_t and uint64_t

Swap upper and lower half: uint8_t .. uint64_t
- **T swap(T val)** 0x12345678 ==> 0x56781234

Rotate Left / Right: uint8_t .. uint64_t
if pos larger than # bits original value is returned.
- **T bitRotateLeft(T value, uint8_t pos)**
- **T bitRotateRight(T value, uint8_t pos)** 

BitFlip: uint8_t .. uint64_t  a.k.a toggle
if pos larger than # bits original value is returned.
- **T bitFlip(T value, uint8_t pos)** flips a single bit at pos

BitRot: uint8_t .. uint64_t
- **T bitRot(T value, float chance = 0.5)** random damage to a single bit of a value,
chance = float 0.0 .. 1.0 that one random bit is toggled. 
**bitRot()** is a function that can be used to mimic single bit errors in communication protocols.  
*Note: a chance of 50% for 2 uint8_t is not equal to 50% chance for 1 uint16_t.*


### 0.1.1 added

How many bits are needed to store / transmit a number?
- **bitsNeededReference(n)** reference implementation for uit to uint64_t.
- **bitsNeeded(n)** A 'recursive strategy' for uint8_t .. uint64_t provides a fast answer. 

The following functions are made as the normal **bitset()** etc do not work for 64 bit.
These functions are optimized for speed for **AVR**, **ESP32** and **ESP8266**. 
- **void bitSet64(uint64 & x, uint8_t bit)** set bit of uint64_t
- **void bitClear64(uint64 & x, uint8_t bit)** clear bit of uint64_t
- **void bitToggle64(uint64 & x, uint8_t bit)** toggle bit of uint64_t
- **void bitWrite64(uint64 & x, uint8_t bit, uint8_t value)** set bit of uint64_t to 0 or 1
- **void bitRead64(uint64 & x, uint8_t bit)** reads bit from uint64_t 

Also added are macro versions of these five functions.
- **mbitSet64(x, bit)** set bit of uint64_t
- **mbitClear64(x, bit)** clear bit of uint64_t
- **mbitToggle64(x, bit)** toggle bit of uint64_t
- **mbitWrite64(x, bit, value)** set bit of uint64_t to 0 or 1
- **mbitRead64(x, bit)** reads bit from uint64_t 


### 0.1.2 added

Added Arduino-CI and unit tests


### 0.1.3 added

- update readme.md
- update unit tests


## BitReverse n bit number

Trick to reverse a number of n bits  ( 0 < n < 32 ).
Could also be done similar with 64 bit and or byte / nybble reverse.

not as fast as a dedicated version.
```cpp
uint32_t bitReverse(uint32_t x, uint8_t n)
{
  uint32_t r = bitReverse(x);
  return r >> (32 - n);
}
```
Could be added in next release...


## Future

- besides **bitRot()** one can also have timing issues when clocking in bits. 
A function could be created to mimic such timing error, by shifting bits from a 
specific position. e.g. 
- parShiftLeft(00001010, 4) ==> 00011010
- bitBurst(00000000, 3) ==>  00111000 any group of 3 bits will toggle.
- bitRot(value, chance = 50%, times = 1) extention...
- bitNoggle(value, bit) - toggle all but one bit. (why?)
- bitSort(value) ==> 00101001 ==> 00000111
- many more :)
- add **bitReverse(uint32_t x, uint8_t n)**
- add **byteReverse24(uint32_t x)** dedicated 24 bit = 3 bytes e.g RGB
- add **byteInverse(uint32_t x)**  (a,b,c,d) => (255-a, 255-b, 255-c, 255-d)

## Operations

See examples.
