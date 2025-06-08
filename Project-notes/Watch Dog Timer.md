# Complete example code from Claude:
```C++
#include <avr/wdt.h>
#include <avr/interrupt.h>

volatile unsigned long last_message_time = 0;
const unsigned long MESSAGE_INTERVAL = 15UL * 60UL * 1000UL; // 15 minutes in milliseconds

void setup() {
  Serial.begin(9600);
  while (!Serial) {
    ; // Wait for serial port to connect (needed for some boards)
  }
  
  Serial.println("Arduino Watchdog Timer - 15 Minute Interval");
  Serial.println("Initializing...");
  
  // Disable interrupts while setting up watchdog
  cli();
  
  // Clear any existing watchdog reset flag
  MCUSR &= ~(1 << WDRF);
  
  // Configure watchdog timer for 8 second timeout in interrupt mode
  WDTCSR |= (1 << WDCE) | (1 << WDE);  // Enable watchdog change
  WDTCSR = (1 << WDIE) | (1 << WDP3) | (1 << WDP0);  // 8 second timeout, interrupt mode
  
  // Re-enable interrupts
  sei();
  
  Serial.println("Setup complete. Message will appear every ~15 minutes.");
  Serial.println("Starting watchdog timer...");
}

void loop() {
  // Main loop can do other tasks
  // The watchdog interrupt will handle the timing
  delay(100);  // Small delay to prevent busy waiting
  
  // Optional: Add a heartbeat to show the system is running
  static unsigned long lastHeartbeat = 0;
  if (millis() - lastHeartbeat > 30000) {  // Every 30 seconds
    Serial.print(".");
    lastHeartbeat = millis();
  }
}

// Watchdog Timer Interrupt Service Routine
ISR(WDT_vect) {
  // Check if 15 minutes have passed using millis()
  unsigned long current_time = millis();
  
  if (current_time - last_message_time >= MESSAGE_INTERVAL) {
    Serial.println(); // New line after dots
    Serial.print("15 minutes elapsed! Uptime: ");
    Serial.print(current_time / 1000 / 60);
    Serial.println(" minutes");
    
    last_message_time = current_time;
  }
}
```

# Arduino Watchdog Timer Code - Line by Line Breakdown

## Header Includes

```cpp
#include <avr/wdt.h>
```

**Purpose**: Includes the AVR watchdog timer library containing macros and functions for watchdog control. **Key macros provided**: `WDTCSR`, `WDCE`, `WDE`, `WDIE`, `WDP0-WDP3`, `WDRF`, `MCUSR`

```cpp
#include <avr/interrupt.h>
```

**Purpose**: Includes AVR interrupt handling library for interrupt service routines and control. **Key macros provided**: `ISR()`, `cli()`, `sei()`, interrupt vector names like `WDT_vect`

## Global Variables

```cpp
volatile unsigned long last_message_time = 0;
```

**Purpose**: Stores the timestamp of the last message output

- `volatile`: Tells compiler this variable can be modified by interrupts, prevents optimization issues
- `unsigned long`: 32-bit unsigned integer (0 to 4,294,967,295) - sufficient for millisecond timestamps
- Initialized to 0 so first message triggers immediately

```cpp
const unsigned long MESSAGE_INTERVAL = 15UL * 60UL * 1000UL;
```

**Purpose**: Defines the 15-minute interval in milliseconds

- `const`: Value cannot be changed after initialization
- `15UL`: 15 as unsigned long literal
- `60UL`: 60 seconds per minute as unsigned long
- `1000UL`: 1000 milliseconds per second as unsigned long
- **Result**: 900,000 milliseconds (exactly 15 minutes)
- `UL` suffix ensures calculation uses unsigned long arithmetic, preventing overflow

## Setup Function

```cpp
void setup() {
```

**Purpose**: Arduino initialization function, runs once at startup

```cpp
Serial.begin(9600);
```

**Purpose**: Initializes serial communication at 9600 baud rate for debugging output

```cpp
while (!Serial) {
  ; // Wait for serial port to connect (needed for some boards)
}
```

**Purpose**: Waits for serial connection to establish

- **Critical for**: Leonardo, Micro, and other boards with native USB
- **Behavior**: Loops until `Serial` evaluates to true (connection established)
- **Empty statement** `;` creates a do-nothing loop body

```cpp
Serial.println("Arduino Watchdog Timer - 15 Minute Interval");
Serial.println("Initializing...");
```

**Purpose**: Debug output to confirm program start

```cpp
cli();
```

**Purpose**: **C**lear **I**nterrupts - disables all interrupts globally

- **Macro expands to**: Assembly instruction that clears the Global Interrupt Enable bit
- **Critical during**: Watchdog configuration to prevent interference
- **Must be paired with**: `sei()` to re-enable interrupts

```cpp
MCUSR &= ~(1 << WDRF);
```

**Purpose**: Clears the Watchdog Reset Flag

- `MCUSR`: **M**CU **S**tatus **R**egister - contains reset source flags
- `WDRF`: **W**atch**d**og **R**eset **F**lag bit position (usually bit 3)
- `(1 << WDRF)`: Creates bitmask with only WDRF bit set
- `~(1 << WDRF)`: Inverts bitmask (all bits 1 except WDRF)
- `&=`: Bitwise AND assignment - clears only the WDRF bit
- **Why needed**: Prevents unintended watchdog behavior from previous resets

```cpp
WDTCSR |= (1 << WDCE) | (1 << WDE);
```

**Purpose**: Enables watchdog timer configuration change mode

- `WDTCSR`: **W**atch**d**og **T**imer **C**ontrol and **S**tatus **R**egister
- `WDCE`: **W**atch**d**og **C**hange **E**nable bit (usually bit 4)
- `WDE`: **W**atch**d**og system reset **E**nable bit (usually bit 3)
- `(1 << WDCE)`: Creates bitmask with WDCE bit set
- `(1 << WDE)`: Creates bitmask with WDE bit set
- `|`: Bitwise OR combines the bitmasks
- `|=`: Bitwise OR assignment - sets both bits
- **Critical timing**: Must be followed immediately by watchdog configuration

```cpp
WDTCSR = (1 << WDIE) | (1 << WDP3) | (1 << WDP0);
```

**Purpose**: Configures watchdog for 8-second timeout in interrupt mode

- `WDIE`: **W**atch**d**og **I**nterrupt **E**nable bit - enables interrupt mode instead of reset
- `WDP3, WDP0`: **W**atch**d**og **P**rescaler bits 3 and 0
- **Prescaler combination**: WDP3=1, WDP2=0, WDP1=0, WDP0=1 → 8.0 seconds
- **Assignment (=)**: Overwrites entire register (clears other bits including WDE)
- **Result**: Watchdog generates interrupt every ~8 seconds without resetting system

### Watchdog Prescaler Values Reference:

|WDP3|WDP2|WDP1|WDP0|Timeout|
|---|---|---|---|---|
|0|0|0|0|16ms|
|0|0|0|1|32ms|
|0|0|1|0|64ms|
|0|0|1|1|125ms|
|0|1|0|0|250ms|
|0|1|0|1|500ms|
|0|1|1|0|1.0s|
|0|1|1|1|2.0s|
|1|0|0|0|4.0s|
|1|0|0|1|8.0s|
|1|0|1|0|Invalid|
|1|0|1|1|Invalid|

```cpp
sei();
```

**Purpose**: **S**et **I**nterrupts - re-enables global interrupts

- **Macro expands to**: Assembly instruction that sets the Global Interrupt Enable bit
- **Required after**: `cli()` to restore normal interrupt operation
- **Enables**: Watchdog timer interrupts to function

```cpp
Serial.println("Setup complete. Message will appear every ~15 minutes.");
Serial.println("Starting watchdog timer...");
```

**Purpose**: Confirmation messages that setup completed successfully

## Main Loop

```cpp
void loop() {
```

**Purpose**: Arduino main execution loop, runs continuously after setup()

```cpp
delay(100);
```

**Purpose**: Pauses execution for 100 milliseconds

- **Prevents**: Busy-waiting that wastes CPU cycles
- **Allows**: Other processes and interrupts to execute efficiently
- **Power saving**: Reduces overall power consumption

```cpp
static unsigned long lastHeartbeat = 0;
```

**Purpose**: Tracks time of last heartbeat output

- `static`: Variable persists between function calls (retains value)
- **Scope**: Local to loop() function but maintains state
- **Initialized once**: Only on first loop() execution

```cpp
if (millis() - lastHeartbeat > 30000) {
```

**Purpose**: Checks if 30 seconds have elapsed since last heartbeat

- `millis()`: Returns milliseconds since Arduino started (wraps after ~49 days)
- **Comparison**: Current time minus last heartbeat time
- `30000`: 30 seconds in milliseconds
- **Handles wraparound**: Unsigned arithmetic naturally handles millis() overflow

```cpp
Serial.print(".");
lastHeartbeat = millis();
```

**Purpose**: Outputs heartbeat dot and updates timestamp

- **Visual indicator**: Shows system is alive between 15-minute messages
- **Updates timestamp**: Resets the 30-second timer

## Interrupt Service Routine

```cpp
ISR(WDT_vect) {
```

**Purpose**: **I**nterrupt **S**ervice **R**outine for watchdog timer

- `ISR()`: Macro that creates interrupt handler function
- `WDT_vect`: Watchdog timer interrupt vector name
- **Triggered**: Every ~8 seconds by watchdog timer
- **Restrictions**: Should be fast, avoid Serial.print, minimize processing

```cpp
unsigned long current_time = millis();
```

**Purpose**: Captures current system time for accurate interval calculation

- **Local variable**: Not volatile since only used within ISR
- **Snapshot**: Prevents time changes during execution

```cpp
if (current_time - last_message_time >= MESSAGE_INTERVAL) {
```

**Purpose**: Checks if 15 minutes (900,000ms) have elapsed

- **Comparison**: Uses unsigned arithmetic for wraparound safety
- `>=`: Ensures message triggers even if slightly over interval
- **Accuracy**: Based on system clock rather than watchdog timer

```cpp
Serial.println();
```

**Purpose**: Outputs newline to separate from heartbeat dots

- **Note**: Serial operations in ISR should be minimal for best practices

```cpp
Serial.print("15 minutes elapsed! Uptime: ");
Serial.print(current_time / 1000 / 60);
Serial.println(" minutes");
```

**Purpose**: Outputs the 15-minute elapsed message with uptime

- `current_time / 1000`: Converts milliseconds to seconds
- `/ 60`: Converts seconds to minutes
- **Integer division**: Truncates fractional minutes

```cpp
last_message_time = current_time;
```

**Purpose**: Updates timestamp for next 15-minute interval calculation

- **Resets timer**: Next message will be 15 minutes from this point
- **Drift correction**: Uses actual current time, not calculated time

## Key Concepts Explained

### Volatile Keyword

- **Purpose**: Prevents compiler optimization of variables modified by interrupts
- **Without volatile**: Compiler might optimize away variable checks
- **With volatile**: Compiler always reads from memory location

### Interrupt Safety

- **ISRs should be fast**: Long operations can interfere with system timing
- **Minimal Serial use**: Serial operations can be slow in ISRs
- **No delay() in ISRs**: delay() doesn't work properly in interrupt context

### Unsigned Arithmetic Wraparound

- **32-bit millis()**: Wraps to 0 after 4,294,967,295 milliseconds (~49.7 days)
- **Subtraction method**: `(current - previous)` works correctly even after wraparound
- **Example**: If previous=4,294,967,200 and current=100 (after wrap), difference is still 900