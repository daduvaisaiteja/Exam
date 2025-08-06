# Exam
Keil
#include <reg51.h>

// Choose half-period: 500 for 1 ms full period, 1000 for 2 ms
#define HALF_PERIOD_US 500

#if HALF_PERIOD_US == 500
    #define TH0_RELOAD 254
    #define TL0_RELOAD 3
#elif HALF_PERIOD_US == 1000
    #define TH0_RELOAD 252
    #define TL0_RELOAD 246
#else
    #error "Unsupported HALF_PERIOD_US value"
#endif

void main(void) {
    // Set P1.0 as output
    P1_0 = 0;

    // Timer0 Mode 1 (16-bit)
    TMOD &= 0xF0;
    TMOD |= 0x01;

    // Load initial timer values
    TH0 = TH0_RELOAD;
    TL0 = TL0_RELOAD;

    // Enable Timer0 interrupt
    ET0 = 1;
    EA  = 1;

    // Start Timer0
    TR0 = 1;

    while (1) {
        // Main loop idle
    }
}

// ISR toggles P1.0 and reloads timer
void Timer0_ISR(void) interrupt 1 {
    P1_0 = ~P1_0;
    TH0 = TH0_RELOAD;
    TL0 = TL0_RELOAD;
}
