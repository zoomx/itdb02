itdb02
Update to V1.3 , support ITDB02-3.2WC and ITDB02-2.4D

V1.2

V1.1

V1.0

VBeta

ITDB02 is a powerful LCD breakout module , it has a 2.4"/3.2" TFT LCD and touch screen , SD card socket.

We will release a library for ITDB02 , the library will include the LCD display function , touch function, and the SD Card read/write function.

Now we have release the first version library for arduino. This version just provide the LCD display functions and touch functions , without SD Card support yet , but we will update the new version to fill with more functions and fix the bugs soon.

We hope everyone who try using this library can give us your suggestion , any advice will be appreciated and help developing the library.

ITDB02(int D8, int D9,int D10, int D11,int D12, int D13,int D14, int D15,int RS, int WR,int CS, int RST)

Enumerating function , define a new class of ITDB02 and assign the pins for the object. Here you can define a LCD ’s pins out - for 8 bit mode.

ITDB02(int D0, int D1,int D2, int D3,int D4, int D5,int D6, int D7,int D8, int D9,int D10, int D11,int D12, int D13,int D14, int D15, int RS, int WR,int CS, int RST)

Enumerating function , define a new class of ITDB02 and assign the pins for the object. Here you can define a LCD ’s pins out - for 16 bit mode.

CleanLCD()

Clean the LCD , plan all the LCD with white color.

void Initial(int Model)

Used in setup() loop , for initial screen settings.

Pant(int sx, int sy, int ex, int ey, int col)

Fill a area with one color , using the sx , sy , ex , ey to set the color area , and the col is the color value.

Setcolor(int FC,int BC)

Set the foreground color and background color . It’s usually used before show char or string.

Dispshowchar(int x, int y, char val)

Show a char in the specified location.

Dispshowstr(int x,int y, char @st)

Show the string , x, y is the start position .

Drawdot(int x, int y)

Draw a 9 pixes dot in the LCD.

Touchpin(int tclk,int tcs,int tdin,int dout, int irq)

Assign pins for touch controller.

TouchInitial()

Start SPI.

TouchGetaddress()

Read the address from the touch controller register. The address will be put in the TP-X and TP-Y global variables.

TouchGetX() Change the TP-X to the coordinate in LCD.

TouchGetY()

Change the TP-Y to the coordinate in LCD.

TouchIRQ()

Determine whether there is touch interrupt.

Demo
Show string
#include <ITDB02.h>

ITDB02 lcd(0,1,2,3,4,5,6,7,8,9,10,11);

void setup()
{
  lcd.Initial();
  lcd.CleanLCD();
}

void loop()
{

  lcd.Setcolor(0x00,0xffff);
  lcd.Dispshowstr(80,300,"iteadstudio.com");

}
Touch
#include <ITDB02.h>

ITDB02 lcd(0,1,2,3,4,5,6,7,8,9,10,11);

void setup()
{
  lcd.Touchpin(14,15,16,18,19);
  lcd.Initial();
  lcd.CleanLCD();
  lcd.TouchInitial();
}

void loop()
{
  char ASCII[16]={
    '0','1','2','3','4','5','6','7','8','9','A','B','C','D','E','F'  };
  char tp;
  unsigned int lx,ly;
  lcd.Setcolor(0,0xffff);
  lcd.Dispshowstr(5,5,"Touch Test Demo");
  lcd.Dispshowstr(130,300,"iteadstudio.com");
  for(;;)
  {
    if(lcd.TouchIRQ()==0)
    {
      lcd.TouchGetaddress();

      lcd.Dispshowchar(3,300,'X');
      lcd.Dispshowchar(10,300,' ');
      tp=ASCII[(lcd.TP_X>>24)&0x0F];
      lcd.Dispshowchar(17,300,tp);
      tp=ASCII[(lcd.TP_X>>16)&0x0F];
      lcd.Dispshowchar(24,300,tp);
      tp=ASCII[(lcd.TP_X>>8)&0x0F];
      lcd.Dispshowchar(31,300,tp);
      tp=ASCII[(lcd.TP_X&0x0F)];
      lcd.Dispshowchar(37,300,tp);

      lcd.Dispshowchar(53,300,'Y');
      lcd.Dispshowchar(60,300,' ');
      tp=ASCII[(lcd.TP_Y>>24)&0x0F];
      lcd.Dispshowchar(67,300,tp);
      tp=ASCII[(lcd.TP_Y>>16)&0x0F];
      lcd.Dispshowchar(74,300,tp);
      tp=ASCII[(lcd.TP_Y>>8)&0x0F];
      lcd.Dispshowchar(81,300,tp);
      tp=ASCII[(lcd.TP_Y&0x0F)];
      lcd.Dispshowchar(88,300,tp);

      lx=lcd.TouchGetX();
      ly=lcd.TouchGetY();
      lcd.Drawdot(lx,ly);
    } 

  }
}
You can fine a more powerful library that made by Henning Karlsen here:

Library: ITDB02_Graph16: http://www.henningkarlsen.com/electronics/a_l_itdb02_graph16.php

Library: ITDB02_Graph: http://www.henningkarlsen.com/electronics/a_l_itdb02_graph.php?1303974731
