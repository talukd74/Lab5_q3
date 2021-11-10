# Lab5_q3


#include <msp430.h>

#define RED LED BIT6
/**
 * main.c
 */
void main(void)
{
    P1DIR |= BIT6;
    WDTCTL = WDTPW | WDTHOLD;   // stop watchdog timer
    TACCR0 = 31250-1;             // PWM Time Period/ frequency (1 KHz)
    TACCTL1 = OUTMOD_7;          // reset/set mode 7 for output signal
    TACCR1 = 6250-1;                // PWM Duty cycle is 75%
    TACTL = TASSEL_2 + MC_1 + ID_3;   // SMCLK and Up Mode then clock divide by 4

    TA0CCTL0 |= CCIE;
    TA0CCTL1 |= CCIE;
    TA0CCTL0 &=~CCIFG;
    TA0CCTL1 &=~CCIFG;

    _enable_interrupts();

    while(1);
}

#pragma vector = TIMER0_A0_VECTOR       //define the interrupt service vector
interrupt void TA0_ISR (void)    // interrupt service routine
{
    P1OUT |=BIT6;
    TA0CCTL0 &=~CCIFG;
}

#pragma vector = TIMER0_A1_VECTOR       //define the interrupt service vector
interrupt void TA1_ISR (void)    // interrupt service routine
{
    P1OUT &=~BIT6;
    TA0CCTL1 &=~CCIFG;
}
