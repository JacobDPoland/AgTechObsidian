# Claude Example code
```c++
#include <iostream>
#include <bitset>
#include <iomanip>
#include <cstdint>

using namespace std;

// Function to print a number in multiple formats
void printNumber(const string& label, int num) {
    cout << label << endl;
    cout << "  Decimal: " << num << endl;
    cout << "  Binary:  " << bitset<8>(num) << endl;
    cout << "  Hex:     0x" << hex << uppercase << num << dec << endl;
    cout << endl;
}

int main() {
    cout << "=== BIT SHIFTING DEMONSTRATION ===" << endl << endl;
    
    // Basic left shift examples
    cout << "LEFT SHIFT (<<) - Multiplies by powers of 2" << endl;
    cout << "Formula: number << n = number * (2^n)" << endl << endl;
    
    int num = 5;  // Starting with 5 (binary: 00000101)
    printNumber("Original number (5):", num);
    
    printNumber("5 << 1 (shift left 1 position):", num << 1);
    printNumber("5 << 2 (shift left 2 positions):", num << 2);
    printNumber("5 << 3 (shift left 3 positions):", num << 3);
    
    cout << "Notice: Left shift by n positions = multiply by 2^n" << endl;
    cout << "5 << 1 = 5 * 2^1 = 10" << endl;
    cout << "5 << 2 = 5 * 2^2 = 20" << endl;
    cout << "5 << 3 = 5 * 2^3 = 40" << endl << endl;
    
    // Basic right shift examples
    cout << "RIGHT SHIFT (>>) - Divides by powers of 2" << endl;
    cout << "Formula: number >> n = number / (2^n)" << endl << endl;
    
    num = 40;  // Starting with 40 (binary: 00101000)
    printNumber("Original number (40):", num);
    
    printNumber("40 >> 1 (shift right 1 position):", num >> 1);
    printNumber("40 >> 2 (shift right 2 positions):", num >> 2);
    printNumber("40 >> 3 (shift right 3 positions):", num >> 3);
    
    cout << "Notice: Right shift by n positions = divide by 2^n (integer division)" << endl;
    cout << "40 >> 1 = 40 / 2^1 = 20" << endl;
    cout << "40 >> 2 = 40 / 2^2 = 10" << endl;
    cout << "40 >> 3 = 40 / 2^3 = 5" << endl << endl;
    
    // Bit manipulation examples
    cout << "BIT MANIPULATION EXAMPLES" << endl << endl;
    
    // Setting specific bits
    cout << "Setting specific bits using (1 << position):" << endl;
    int flags = 0;  // Start with all bits 0
    printNumber("Initial flags:", flags);
    
    flags |= (1 << 2);  // Set bit 2
    printNumber("After setting bit 2 (flags |= (1 << 2)):", flags);
    
    flags |= (1 << 5);  // Set bit 5
    printNumber("After setting bit 5 (flags |= (1 << 5)):", flags);
    
    flags |= (1 << 7);  // Set bit 7
    printNumber("After setting bit 7 (flags |= (1 << 7)):", flags);
    
    // Clearing specific bits
    cout << "Clearing specific bits using ~(1 << position):" << endl;
    flags &= ~(1 << 2);  // Clear bit 2
    printNumber("After clearing bit 2 (flags &= ~(1 << 2)):", flags);
    
    // Checking specific bits
    cout << "Checking if specific bits are set:" << endl;
    bool bit5Set = (flags & (1 << 5)) != 0;
    bool bit2Set = (flags & (1 << 2)) != 0;
    cout << "Is bit 5 set? " << (bit5Set ? "YES" : "NO") << endl;
    cout << "Is bit 2 set? " << (bit2Set ? "YES" : "NO") << endl << endl;
    
    // Practical examples
    cout << "PRACTICAL EXAMPLES" << endl << endl;
    
    // Powers of 2
    cout << "Creating powers of 2:" << endl;
    for (int i = 0; i <= 7; i++) {
        int power = 1 << i;
        cout << "2^" << i << " = (1 << " << i << ") = " << power 
             << " (binary: " << bitset<8>(power) << ")" << endl;
    }
    cout << endl;
    
    // Register-style operations (like Arduino)
    cout << "Arduino-style register operations:" << endl;
    uint8_t CONTROL_REG = 0b00000000;  // Simulated control register
    
    const int ENABLE_BIT = 0;
    const int SPEED_BIT1 = 1;
    const int SPEED_BIT2 = 2;
    const int INTERRUPT_BIT = 7;
    
    cout << "Initial register: " << bitset<8>(CONTROL_REG) << endl;
    
    // Enable device
    CONTROL_REG |= (1 << ENABLE_BIT);
    cout << "After enabling:   " << bitset<8>(CONTROL_REG) << endl;
    
    // Set speed bits (binary 11 = 3)
    CONTROL_REG |= (1 << SPEED_BIT1) | (1 << SPEED_BIT2);
    cout << "After set speed:  " << bitset<8>(CONTROL_REG) << endl;
    
    // Enable interrupt
    CONTROL_REG |= (1 << INTERRUPT_BIT);
    cout << "After interrupt:  " << bitset<8>(CONTROL_REG) << endl;
    
    // Clear speed bits but keep others
    CONTROL_REG &= ~((1 << SPEED_BIT1) | (1 << SPEED_BIT2));
    cout << "After clear speed:" << bitset<8>(CONTROL_REG) << endl;
    
    cout << endl << "=== DEMONSTRATION COMPLETE ===" << endl;
    
    return 0;
}
```

# Output:
```
=== BIT SHIFTING DEMONSTRATION ===

LEFT SHIFT (<<) - Multiplies by powers of 2
Formula: number << n = number * (2^n)

Original number (5):
  Decimal: 5
  Binary:  00000101
  Hex:     0x5

5 << 1 (shift left 1 position):
  Decimal: 10
  Binary:  00001010
  Hex:     0xA

5 << 2 (shift left 2 positions):
  Decimal: 20
  Binary:  00010100
  Hex:     0x14

5 << 3 (shift left 3 positions):
  Decimal: 40
  Binary:  00101000
  Hex:     0x28

Notice: Left shift by n positions = multiply by 2^n
5 << 1 = 5 * 2^1 = 10
5 << 2 = 5 * 2^2 = 20
5 << 3 = 5 * 2^3 = 40

RIGHT SHIFT (>>) - Divides by powers of 2
Formula: number >> n = number / (2^n)

Original number (40):
  Decimal: 40
  Binary:  00101000
  Hex:     0x28

40 >> 1 (shift right 1 position):
  Decimal: 20
  Binary:  00010100
  Hex:     0x14

40 >> 2 (shift right 2 positions):
  Decimal: 10
  Binary:  00001010
  Hex:     0xA

40 >> 3 (shift right 3 positions):
  Decimal: 5
  Binary:  00000101
  Hex:     0x5

Notice: Right shift by n positions = divide by 2^n (integer division)
40 >> 1 = 40 / 2^1 = 20
40 >> 2 = 40 / 2^2 = 10
40 >> 3 = 40 / 2^3 = 5

BIT MANIPULATION EXAMPLES

Setting specific bits using (1 << position):
Initial flags:
  Decimal: 0
  Binary:  00000000
  Hex:     0x0

After setting bit 2 (flags |= (1 << 2)):
  Decimal: 4
  Binary:  00000100
  Hex:     0x4

After setting bit 5 (flags |= (1 << 5)):
  Decimal: 36
  Binary:  00100100
  Hex:     0x24

After setting bit 7 (flags |= (1 << 7)):
  Decimal: 164
  Binary:  10100100
  Hex:     0xA4

Clearing specific bits using ~(1 << position):
After clearing bit 2 (flags &= ~(1 << 2)):
  Decimal: 160
  Binary:  10100000
  Hex:     0xA0

Checking if specific bits are set:
Is bit 5 set? YES
Is bit 2 set? NO

PRACTICAL EXAMPLES

Creating powers of 2:
2^0 = (1 << 0) = 1 (binary: 00000001)
2^1 = (1 << 1) = 2 (binary: 00000010)
2^2 = (1 << 2) = 4 (binary: 00000100)
2^3 = (1 << 3) = 8 (binary: 00001000)
2^4 = (1 << 4) = 16 (binary: 00010000)
2^5 = (1 << 5) = 32 (binary: 00100000)
2^6 = (1 << 6) = 64 (binary: 01000000)
2^7 = (1 << 7) = 128 (binary: 10000000)

Arduino-style register operations:
Initial register: 00000000
After enabling:   00000001
After set speed:  00000111
After interrupt:  10000111
After clear speed:10000001

=== DEMONSTRATION COMPLETE ===

```