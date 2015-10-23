Update of 23/10/2015

This RFM69 Library merges the current and official one of Felix Rusus dated of the 2nd of October 2015 (See https://github.com/LowPowerLab/RFM69) and the patch for the Ethernet / RFM69 interrupt when a RFM69 transceiver is used with and Ethernet Shield; issue solved by kiwisincebirth  (see https://github.com/kiwisincebirth/RFM69).
It also include an extension of the Ctrl Byte (see RFM69.h) which is used by the RFM69_SessionKey library made by dewoodruff (see https:https://github.com/dewoodruff/RFM69_SessionKey).
The definition ….
// TWS: define CTLbyte bits
#define RFM69_CTL_SENDACK   0x80
#define RFM69_CTL_REQACK    0x40

Should also include ….
#define RFM69_CTL_RESERVE1  0x20
#define RFM69_CTL_RESERVE2  0x10
#define RFM69_CTL_EXT1      0x08   // reserve for RFM69 derived classes to use
#define RFM69_CTL_EXT2      0x04
#define RFM69_CTL_EXT3      0x02
#define RFM69_CTL_EXT4      0x01

IMPORTANT:
For using the RFM69_SessionKey library with and Ethernet Shield, the RFM69_SessionKey.cpp file has to include the same correction as the one of the kiwisincebirth regarding the receiveDone() function.

This function should be replaced by:

//=============================================================================
//  receiveDone() - Added check so if session key is enabled, incoming key is checked 
//                  against the stored key for this session. Returns false if not matched
//=============================================================================
 bool RFM69_SessionKey::receiveDone() {
//ATOMIC_BLOCK(ATOMIC_FORCEON)
//{
  uint8_t rd_SREG = SREG; // **1 Interrupt Control
  noInterrupts(); // re-enabled in unselect() via setMode() or via receiveBegin()
  if (_mode == RF69_MODE_RX && PAYLOADLEN > 0)
  {
    // if session key on and keys don't match
    // return false, as if nothing was even received
    if (sessionKeyEnabled() && INCOMING_SESSION_KEY != SESSION_KEY) {
      interrupts(); // explicitly re-enable interrupts
      receiveBegin();
      return false;
    }
    setMode(RF69_MODE_STANDBY); // enables interrupts
    SREG = rd_SREG; // **1 Interrupt Control - Restore interrupts
    return true;
  }
  else if (_mode == RF69_MODE_RX) // already in RX no payload yet
  {
    SREG = rd_SREG; // **1 Interrupt Control - Restore interrupts
//    interrupts(); // **1 Interrupt Control - Restore interrupts
    return false;
  }
  receiveBegin();
  SREG = rd_SREG; // **1 Interrupt Control - Restore interrupts
  return false;
}

Robert
