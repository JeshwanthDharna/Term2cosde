/*
DESCRIPTION : This program communicates with the USART terminal using a baud rate of 56,000 when thewhen then user interacts with the joystick then MCU will send ASCII characters as UP,DOWN,LEFT,RIGHT and CLICK upon pressing on which direction the user pressed and when the user writes UP from the terminal of the MCU then recives in terminal as UP Pressed and when sends again it recives as UP Released.It works all the directions including click. And program also counts no. of times button pressed.
*/
//VARIABLE DECLARATIONS
void initialize_USART1();        // Sub function which initializes the registers to enable USART1
void joystick_configuration();  // Sub function which initializes the joystick
void counters();                // Sub function for counters
//Reciving part
unsigned long int rdata[100]; // Container for received data
unsigned long int r = 0; //rdata counter
//UP
unsigned long int PD4_state = 0;
unsigned long int PD4_flag = 0;
unsigned long int PD4_UartFlag = 0;
//DOWN
unsigned long int PB5_state = 0;
unsigned long int PB5_flag = 0;
unsigned long int PB5_UartFlag = 0;
//LEFT
unsigned long int PD2_state = 0;
unsigned long int PD2_flag = 0;
unsigned long int PD2_UartFlag = 0;
//RIGHT
unsigned long int PA6_state = 0;
unsigned long int PA6_flag = 0;
unsigned long int PA6_UartFlag = 0;
//CLICK
unsigned long int PC13_state = 0;
unsigned long int PC13_flag = 0;
unsigned long int PC13_UartFlag = 0;

#define HIGH 1
#define LOW 0

#define STATE3 3
#define STATE2 2
#define STATE1 1
#define STATE0 0
unsigned long int i = 0; // message counter
unsigned long int joy[20] = {'J','o','y','s','t','i','c','k',' ','P','r','e','s','s','e','s',':',0x0D,0x0A};
unsigned long int c[5] = {':',' '};

unsigned long int up[15] = {'U','P',0x0D,0x0A};
unsigned long int down[15] = {'D','O','W','N',0x0D,0x0A};
unsigned long int left[15] = {'L','E','F','T',0x0D,0x0A};
unsigned long int right[15] = {'R','I','G','H','T',0x0D,0x0A};
unsigned long int click[15] = {'C','L','I','C','K',0x0D,0x0A};
// Pressed Messgaes
unsigned long int up_p[20] = {'U','P',' ','P','r','e','s','s','e','d',0x0D,0x0A};
unsigned long int down_p[20] = {'D','N',' ','P','r','e','s','s','e','d',0x0D,0x0A};
unsigned long int left_p[20] = {'L','T',' ','P','r','e','s','s','e','d',0x0D,0x0A};
unsigned long int right_p[20] = {'R','T',' ','P','r','e','s','s','e','d',0x0D,0x0A};
unsigned long int click_p[20] = {'C','K',' ','P','r','e','s','s','e','d',0x0D,0x0A};
//Realeased Messages
unsigned long int up_r[20] = {'U','P',' ','R','e','l','e','a','s','e','d',0x0D,0x0A};
unsigned long int down_r[20] = {'D','N',' ','R','e','l','e','a','s','e','d',0x0D,0x0A};
unsigned long int left_r[20] = {'L','T',' ','R','e','l','e','a','s','e','d',0x0D,0x0A};
unsigned long int right_r[20] = {'R','T',' ','R','e','l','e','a','s','e','d',0x0D,0x0A};
unsigned long int click_r[20] = {'C','K',' ','R','e','l','e','a','s','e','d',0x0D,0x0A};
//Counters
unsigned int up_counter = 0;
unsigned int down_counter = 0;
unsigned int left_counter = 0;
unsigned int right_counter = 0;
unsigned int click_counter = 0;
unsigned long int d0 = 0,d1 = 0;
//**************************************************************************************************
//MAIN FUNCTION
void main()
       {
        initialize_USART1();               // Call sub function to initialize USART1
        joystick_configuration();          // Call sub function to initialize joystick
        GPIOE_ODR = 0x0000;

    for(;;)
    {
     if(((USART1_SR & (1<<5))== 0x20))  // Check RXNE in USART1 Status Register.
     {
      rdata[r] = USART1_DR;    //read data from receiver data register
      r++;

      if(rdata[0] == 'U' || rdata[0] == 'u')   // checking if UP is send in the terminal
      {
        if(rdata[1] == 'P'|| rdata[1] == 'p')
          {
          r = 0;
          rdata[0] = ' ';
          rdata[1] = ' ';

          if(PD4_UartFlag == STATE0) PD4_UartFlag = STATE1;
          if(PD4_UartFlag == STATE2) PD4_UartFlag = STATE3;
          }
       }

       if(rdata[0] == 'D' || rdata[0] == 'd')   // checking if DN is send in the terminal
       {
        if(rdata[1] == 'N'|| rdata[1] == 'n')
          {
          r = 0;
          rdata[0] = ' ';
          rdata[1] = ' ';

          if(PB5_UartFlag == STATE0) PB5_UartFlag = STATE1;
          if(PB5_UartFlag == STATE2) PB5_UartFlag = STATE3;
          }
       }

       if(rdata[0] == 'L' || rdata[0] == 'l')   // checking if LT is send in the terminal
       {
        if(rdata[1] == 'T'|| rdata[1] == 't')
          {
          r = 0;
          rdata[0] = ' ';
          rdata[1] = ' ';

          if(PD2_UartFlag == STATE0) PD2_UartFlag = STATE1;
          if(PD2_UartFlag == STATE2) PD2_UartFlag = STATE3;
          }
       }

       if(rdata[0] == 'R' || rdata[0] == 'r')   // checking if RT is send in the terminal
       {
        if(rdata[1] == 'T'|| rdata[1] == 't')
          {
          r = 0;
          rdata[0] = ' ';
          rdata[1] = ' ';

          if(PA6_UartFlag == STATE0) PA6_UartFlag = STATE1;
          if(PA6_UartFlag == STATE2) PA6_UartFlag = STATE3;
          }
       }
       if(rdata[0] == 'C' || rdata[0] == 'c')   // checking if CK is send in the terminal
       {
        if(rdata[1] == 'K'|| rdata[1] == 'k')
          {
          r = 0;
          rdata[0] = ' ';
          rdata[1] = ' ';

          if(PC13_UartFlag == STATE0) PC13_UartFlag = STATE1;
          if(PC13_UartFlag == STATE2) PC13_UartFlag = STATE3;
          }
       }
       counters();                       // Call sub function for counters
     }
//UP operation
    if(GPIOD_IDR.B4 == LOW && PD4_state == LOW)   // UP(PD4) button pressed
      {
         PD4_state = 1;  //  button state updated to 1
         PD4_flag = 1; // Rising edge, was a 0 and is now a 1
         up_counter = up_counter + 1;   // counting up pressed
       }
    if(GPIOD_IDR.B4 == HIGH && PD4_state == HIGH)   // UP(PD4) button releaed
       {
         PD4_state = 0; // button state updated to 0
         PD4_flag = 2; // Rising edge, was a 0 and is now a 1
       }
    if(PD4_flag == 1) // Printing UP when pressed on joystick(PD4)
         {
          for(i = 0; i < 15; i++)
          {
           while(USART1_SR.TC == 0){}
           USART1_DR = up[i];
          }
         }
    if(PD4_UartFlag == STATE1) // Printing UP Pressed when UP sent in the terminal
         {
          for(i = 0; i < 15; i++)
          {
           while(USART1_SR.TC == 0){}
           USART1_DR = up_p[i];
          }
         }
    if(PD4_flag == 1 || PD4_UartFlag == STATE1)  // If PD4 pressed or UP sent in terminal
       {
         PD4_flag = 0;
         PD4_UartFlag = STATE2;
         GPIOE_ODR = 0b10001000 << 8;   // PE11 and PE15 will turn on
       }
       if(PD4_UartFlag == STATE3) // Printing UP relesed when UP sent again in the terminal
         {
          for(i = 0; i < 15; i++)
          {
           while(USART1_SR.TC == 0){}
           USART1_DR = up_r[i];
          }
         }
    if(PD4_flag == 2 || PD4_UartFlag == STATE3) // Was pressed, now released
        {
         PD4_UartFlag = STATE0;
         PD4_flag = 0;
         GPIOE_ODR = 0x0000;
        }
//Down operation
    if(GPIOB_IDR.B5 == LOW && PB5_state == LOW)   // DOWN(PB5) button pressed
      {
         PB5_state = 1;  //  button state updated to 1
         PB5_flag = 1; // Rising edge, was a 0 and is now a 1
         down_counter = down_counter + 1; // counting down pressed
       }
    if(GPIOB_IDR.B5 == HIGH && PB5_state == HIGH)   // DOWN(PB5) button releaed
       {
         PB5_state = 0; // button state updated to 0
         PB5_flag = 2; // Rising edge, was a 0 and is now a 1
       }
    if(PB5_flag == 1) // Printing DOWN when pressed on joystick(PB5)
         {
          for(i = 0; i < 15; i++)
          {
           while(USART1_SR.TC == 0){}
           USART1_DR = down[i];
          }
         }
    if(PB5_UartFlag == STATE1) // Printing DOWN Pressed when DN sent in the terminal
         {
          for(i = 0; i < 15; i++)
          {
           while(USART1_SR.TC == 0){}
           USART1_DR = down_p[i];
          }
         }
    if(PB5_flag == 1 || PB5_UartFlag == STATE1) // If PB5 pressed or DN sent in terminal
       {
         PB5_flag = 0;
         PB5_UartFlag = STATE2;
         GPIOE_ODR = 0b00010001 << 8;  // PE8 & PE12 will turn on
       }
       if(PB5_UartFlag == STATE3) // Printing DOWN relesed when DN sent again in the terminal
         {
          for(i = 0; i < 15; i++)
          {
           while(USART1_SR.TC == 0){}
           USART1_DR = down_r[i];
          }
         }
    if(PB5_flag == 2 || PB5_UartFlag == STATE3) // Was pressed, now released
        {
         PB5_UartFlag = STATE0;
         PB5_flag = 0;
         GPIOE_ODR = 0x0000;
        }
//Left operation
    if(GPIOD_IDR.B2 == LOW && PD2_state == LOW) // LEFT(PD2) button pressed
      {
         PD2_state = 1;  //  button state updated to 1
         PD2_flag = 1; // Rising edge, was a 0 and is now a 1
         left_counter = left_counter + 1; // counting left pressed
       }
    if(GPIOD_IDR.B2 == HIGH && PD2_state == HIGH) // LEFT(PD2) button releaed
       {
         PD2_state = 0; // button state updated to 0
         PD2_flag = 2; // Rising edge, was a 0 and is now a 1
       }
    if(PD2_flag == 1) // Printing LEFT when pressed on joystick(PD2)
         {
          for(i = 0; i < 15; i++)
          {
           while(USART1_SR.TC == 0){}
           USART1_DR = left[i];
          }
         }
    if(PD2_UartFlag == STATE1) // Printing LEFT Pressed when LT sent in the terminal
         {
          for(i = 0; i < 15; i++)
          {
           while(USART1_SR.TC == 0){}
           USART1_DR = left_p[i];
          }
         }
    if(PD2_flag == 1 || PD2_UartFlag == STATE1) // If PD2 pressed or LT sent in terminal
       {
         PD2_flag = 0;
         PD2_UartFlag = STATE2;
         GPIOE_ODR = 0b01100000 << 8; // PE13 & PE14 will turn on
       }
       if(PD2_UartFlag == STATE3) // Printing LEFT relesed when LT sent again in the terminal
         {
          for(i = 0; i < 15; i++)
          {
           while(USART1_SR.TC == 0){}
           USART1_DR = left_r[i];
          }
         }
    if(PD2_flag == 2 || PD2_UartFlag == STATE3) // Was pressed, now released
        {
         PD2_UartFlag = STATE0;
         PD2_flag = 0;
         GPIOE_ODR = 0x0000;
        }
//Right operation
    if(GPIOA_IDR.B6 == LOW && PA6_state == LOW) //RIGHT(PA6) button pressed
      {
         PA6_state = 1;  //  button state updated to 1
         PA6_flag = 1; // Rising edge, was a 0 and is now a 1
         right_counter = right_counter + 1;  // counting right pressed
       }
    if(GPIOA_IDR.B6 == HIGH && PA6_state == HIGH)   // RIGHT(PA6) button releaed
       {
         PA6_state = 0; // button state updated to 0
         PA6_flag = 2; // Rising edge, was a 0 and is now a 1
       }
    if(PA6_flag == 1) // Printing RIGHT when pressed on joystick(PA6)
         {
          for(i = 0; i < 15; i++)
          {
           while(USART1_SR.TC == 0){}
           USART1_DR = right[i];
          }
         }
    if(PA6_UartFlag == STATE1) // Printing RIGHT Pressed when RT sent in the terminal
         {
          for(i = 0; i < 15; i++)
          {
           while(USART1_SR.TC == 0){}
           USART1_DR = right_p[i];
          }
         }
    if(PA6_flag == 1 || PA6_UartFlag == STATE1)// If PA6 pressed or RT sent in terminal
       {
         PA6_flag = 0;
         PA6_UartFlag = STATE2;
         GPIOE_ODR = 0b0110 << 8; // PE9 & PE10 will turn on
       }
       if(PA6_UartFlag == STATE3) // Printing RIGHT relesed when RT sent again in the terminal
         {
          for(i = 0; i < 16; i++)
          {
           while(USART1_SR.TC == 0){}
           USART1_DR = right_r[i];
          }
         }
    if(PA6_flag == 2 || PA6_UartFlag == STATE3) // Was pressed, now released
        {
         PA6_UartFlag = STATE0;
         PA6_flag = 0;
         GPIOE_ODR = 0x0000;
        }
//Click operation
    if(GPIOC_IDR.B13 == LOW && PC13_state == LOW) //CLICK(PC13) button pressed
      {
         PC13_state = 1; //  button state updated to 1
         PC13_flag = 1; // Rising edge, was a 0 and is now a 1
         click_counter = click_counter + 1;  // counting click pressed
       }
    if(GPIOC_IDR.B13 == HIGH && PC13_state == HIGH) //CLICK(PC13) button releaed
       {
         PC13_state = 0; // button state updated to 0
         PC13_flag = 2; // Rising edge, was a 0 and is now a 1
       }
    if(PC13_flag == 1) // Printing CLICK when pressed on joystick(PC13)
         {
          for(i = 0; i < 15; i++)
          {
           while(USART1_SR.TC == 0){}
           USART1_DR = click[i];
          }
         }
    if(PC13_UartFlag == STATE1) // Printing CLICK when CK sent in the terminal
         {
          for(i = 0; i < 15; i++)
          {
           while(USART1_SR.TC == 0){}
           USART1_DR = click_p[i];
          }
         }
    if(PC13_flag == 1 || PC13_UartFlag == STATE1)  // If PC13 pressed or CK sent in terminal
       {
         PC13_flag = 0;
         PC13_UartFlag = STATE2;
         GPIOE_ODR = 0b01000010 << 8; //PE9 & PE14 will turn on
       }
       if(PC13_UartFlag == STATE3) // Printing CLICK relesed when CK sent again in the terminal
         {
          for(i = 0; i < 16; i++)
          {
           while(USART1_SR.TC == 0){}
           USART1_DR = click_r[i];
          }
         }
    if(PC13_flag == 2 || PC13_UartFlag == STATE3) // Was pressed, now released
        {
         PC13_UartFlag = STATE0;
         PC13_flag = 0;
         GPIOE_ODR = 0x0000;
        }

    }
}
//**************************************************************************************************
//SUB FUNCTIONS
void joystick_configuration() // Sub function which initializes the registers to enable Joysticks buttons
         {
        RCC_APB2ENR |= 1 << 2;  // Enabling GPIOA clock
        GPIOA_CRL |= 0X04000000; // Configure PA6 (Right) as an input

        RCC_APB2ENR |= 1 << 3;  // Enabling GPIOB clock
        GPIOB_CRL |= 0X00400000; // Configure PB5(Down) as an input

        RCC_APB2ENR |= 1 << 4;  // Enabling GPIOC clock
        GPIOC_CRH |= 0X00400000; // Configure PC13 (Click) as an input

        RCC_APB2ENR |= 1 << 5;  // Enabling GPIOD clock
        GPIOD_CRL |= 0x00040400;  // Configure PD2 & PD4 (Left & up) as an input

        RCC_APB2ENR |= 1 << 6;  // Enabling GPIOE clock
        GPIOE_CRH = 0x33333333; // Configure Port E as an output
        }
void initialize_USART1() // Sub function which initializes the registers to enable USART1
       {
        RCC_APB2ENR |= 1;                 // Enable clock for Alt. Function. USART1 uses AF for PA9/PA10
        AFIO_MAPR=0X0F000000;             // Do not mask PA9 and PA10 (becaue we are using for USART)
        RCC_APB2ENR |= 1<<2;              // Enable clock for GPIOA
        GPIOA_CRH &= ~(0xFF << 4);        // Clear PA9, PA10
        GPIOA_CRH |= (0x0B << 4);         // USART1 Tx (PA9) output push-pull
        GPIOA_CRH |= (0x04 << 8);         // USART1 Rx (PA10) input floating
        RCC_APB2ENR |= 1<<14;             // enable clock for USART1
        USART1_BRR=0X00000506;            // Set baud rate to 56000
        USART1_CR1 &= ~(1<<12);          // Force 8 data bits. M bit is set to 0.
        USART1_CR2 &= ~(3<<12);          // Force 1 stop bit
        USART1_CR3 &= ~(3<<8);           // Force no flow control and no DMA for USART1
        USART1_CR1 &= ~(3<<9);           // Force no parity and no parity control
        USART1_CR1 |= 3<<2;              // RX, TX enable
        //The following two instructions can also be used to enable RX and TX manually
        //USART1_CR1.TE=1; //TX enable
        //USART1_CR1.RE=1; //RX enable
        USART1_CR1 |= 1<<13;            // USART1 enable. This is done after configuration is complete
        Delay_ms(100);                  // Wait for USART to complete configuration and enable. This is
                                       // not always necessary, but good practice.
       }
// Button counters
// Sub function which counts the buttons pressed or sent in the terminal
void counters()
     {
       if(rdata[0] == 'Q'|| rdata[0] == 'q')
        {
        r = 0;
        rdata[0] = ' ';
        while (!(USART1_SR & (1<<7)) == 0x80) {} // transmit the received data
        for(i = 0; i < 20; i++)
          {
           while(USART1_SR.TC == 0){}
           USART1_DR = joy[i]; //Printing Joystick Presses:
          }
        //UP Countetr
        for(i = 0; i < 3; i++)
          {
           while(USART1_SR.TC == 0){}
           USART1_DR = up[i];
          }
          for(i = 0; i < 2; i++)
          {
           while(USART1_SR.TC == 0){}
           USART1_DR = c[i];
          }
        d0 = up_counter % 10;
        d1 = ((up_counter - d0)/10) % 10;
        USART1_DR = d1 + 48;
        while(USART1_SR.TC == 0){}
        USART1_DR = d0 + 48;
        while(USART1_SR.TC == 0){}
        USART1_DR = 0x0D;
        while(USART1_SR.TC == 0){}
        USART1_DR = 0x0A;
        //DOWN Counter
        for(i = 0; i < 5; i++)
          {
           while(USART1_SR.TC == 0){}
           USART1_DR = down[i];
          }
          for(i = 0; i < 2; i++)
          {
           while(USART1_SR.TC == 0){}
           USART1_DR = c[i];
          }
        d0 = down_counter % 10;
        d1 = ((down_counter - d0)/10) % 10;
        USART1_DR = d1 + 48;
        while(USART1_SR.TC == 0){}
        USART1_DR = d0 + 48;
        while(USART1_SR.TC == 0){}
        USART1_DR = 0x0D;
        while(USART1_SR.TC == 0){}
        USART1_DR = 0x0A;
        //LEFT Counter
        for(i = 0; i < 5; i++)
          {
           while(USART1_SR.TC == 0){}
           USART1_DR = left[i];
          }
          for(i = 0; i < 2; i++)
          {
           while(USART1_SR.TC == 0){}
           USART1_DR = c[i];
          }
        d0 = left_counter % 10;
        d1 = ((left_counter - d0)/10) % 10;
        USART1_DR = d1 + 48;
        while(USART1_SR.TC == 0){}
        USART1_DR = d0 + 48;
        while(USART1_SR.TC == 0){}
        USART1_DR = 0x0D;
        while(USART1_SR.TC == 0){}
        USART1_DR = 0x0A;
        //RIGHT Counter
        for(i = 0; i < 6; i++)
          {
           while(USART1_SR.TC == 0){}
           USART1_DR = right[i];
          }
          for(i = 0; i < 2; i++)
          {
           while(USART1_SR.TC == 0){}
           USART1_DR = c[i];
          }
        d0 = right_counter % 10;
        d1 = ((right_counter - d0)/10) % 10;
        USART1_DR = d1 + 48;
        while(USART1_SR.TC == 0){}
        USART1_DR = d0 + 48;
        while(USART1_SR.TC == 0){}
        USART1_DR = 0x0D;
        while(USART1_SR.TC == 0){}
        USART1_DR = 0x0A;
        //CLICK Counter
        for(i = 0; i < 6; i++)
          {
           while(USART1_SR.TC == 0){}
           USART1_DR = click[i];
          }
          for(i = 0; i < 2; i++)
          {
           while(USART1_SR.TC == 0){}
           USART1_DR = c[i];
          }
        d0 = click_counter % 10;
        d1 = ((click_counter - d0)/10) % 10;
        USART1_DR = d1 + 48;
        while(USART1_SR.TC == 0){}
        USART1_DR = d0 + 48;
        while(USART1_SR.TC == 0){}
        USART1_DR = 0x0D;
        while(USART1_SR.TC == 0){}
        USART1_DR = 0x0A;
        }
     }
