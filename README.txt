/* From ROB: 4/10/2016
  This RFM69 library implements the SPI TRANSACTION patch that overcomes multiple SPI devices deadlock.
  See https://lowpowerlab.com/forum/moteino/moteino-w5100-ethernet-spi-support-spi_has_transaction/msg5132/#msg5132 
  It includes a work around to make these changes compatible with ESP8266 micro-controllers.
  Despite the fact that there is no version management, the source code reference of this library is the one downloaded 
  from the https://github.com/LowPowerLab/RFM69 the 4/10/2016
  It also add extra definition introduced by TWS for the Control Byte used by secure RFM_SessionKey library.
/* From ROB: 15/08/2017 
 The same issue is encounerd with the ESP32, and extra check is done to avoid the SPI.usingInterrupt(_interruptNum) in case  of a EPS32
/* From ROB: 21/05/2017 
   Return false status if no RFM transciever is installed by checking valid value of the REG_IRQFLAGS1 register
   
    All modifications are surrounded by !!! ROB.   */
