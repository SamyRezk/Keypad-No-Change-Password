# Keypad-No-Change-Password
// Compiler : MikroC
// Device : Pic16F877A
// Keypad module connections
char  keypadPort at PORTB;
// End Keypad module connections

// LCD module connections
sbit LCD_RS at RD4_bit;
sbit LCD_EN at RD5_bit;
sbit LCD_D4 at RD0_bit;
sbit LCD_D5 at RD1_bit;
sbit LCD_D6 at RD2_bit;
sbit LCD_D7 at RD3_bit;

sbit LCD_RS_Direction at TRISD4_bit;
sbit LCD_EN_Direction at TRISD5_bit;
sbit LCD_D4_Direction at TRISD0_bit;
sbit LCD_D5_Direction at TRISD1_bit;
sbit LCD_D6_Direction at TRISD2_bit;
sbit LCD_D7_Direction at TRISD3_bit;
// End LCD module connections
int i ;
char password[4];
char get_password()
{
Loop:
lcd_cmd(_lcd_clear);
lcd_out(1 , 1 , "Enter Password");
  for(i =0 ; i< 3 ; i++ ){
     while(password[i] == 0)
     {
      password[i] = Keypad_Key_Click();
      }
      if(password[i] == 9) password[i] = '1' ;
      if(password[i] == 10) password[i] = '2' ;
      if(password[i] == 11) password[i] = '3' ;
      Lcd_Chr(2 , i+1 , '*');
     }
if(strcmp(password , "123") == 0){return 1;}
else {
        lcd_cmd(_lcd_clear);
        goto Loop ;
     }
}
void main()
{
  trisc=0;
  portc = 0;
  keypad_init();
  lcd_init();
  lcd_cmd(_lcd_cursor_off);
  get_password();
   while(1)
   {
    lcd_cmd(_lcd_clear);
    lcd_out(1 , 4 , "flash Program");
    Portc.b0 =~ PortC.B0 ;
    delay_ms(1000);
 }
}
