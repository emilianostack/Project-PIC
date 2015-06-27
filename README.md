# Project-PIC
~le Cornomentro~
// LCD module connections
sbit LCD_RS at RE2_bit;
sbit LCD_EN at RE1_bit;
sbit LCD_D4 at RD4_bit;
sbit LCD_D5 at RD5_bit;
sbit LCD_D6 at RD6_bit;
sbit LCD_D7 at RD7_bit;

sbit LCD_RS_Direction at TRISE2_bit;
sbit LCD_EN_Direction at TRISE1_bit;
sbit LCD_D4_Direction at TRISD4_bit;
sbit LCD_D5_Direction at TRISD5_bit;
sbit LCD_D6_Direction at TRISD6_bit;
sbit LCD_D7_Direction at TRISD7_bit;
// End LCD module connections


void LimparTela(){

  Lcd_Init();                        // Initialize LCD

  //Visão do Display
  //----------------//
  //VLT: 99.99-99.99//
  //TEMPO : 99 . 99 //
  //----------------//

  Lcd_Cmd(_LCD_CLEAR);               // Clear display
  Lcd_Cmd(_LCD_CURSOR_OFF);          // Cursor off
  Lcd_Out(1,1,"VLT: ");              // Write text in first row
  Lcd_Out(2,1,"TEMPO: ");            // Write text in first row

}





char miliSegundo_txt[2];
void MostrarMilisegundos(){
     int milisegundo = 0;
     
       // while (milisegundo < 10) {
                          
                          milisegundo++;
                          /*WordToStr(milisegundo,miliSegundo_txt);
                          Lcd_Out(1,8,miliSegundo_txt);
                          //MostrarCentesimoSegundo(milisegundo);
                          //WaitForMiliSec(1);
                          InitTimer0();
                          InterruptMilisecond();
                          */
        //}
}

char segundo_txt[2];
void MostrarSegundo(int seg){

    WordToStr(seg,segundo_txt);
    //MostrarMilisegundos();
    Lcd_out(2,8,segundo_txt);
    Delay_ms(100);
    
}

  int segundo = 0;
void main(){
   TRISB.RB2 = 1;
   PORTB.RB2 = 1;
   TRISB.RB0 = 1;
   PORTB.RB0 = 1;
   ADCON1 = 0x07;   //0x8E or 0x07

   LimparTela();

    while(1){

          /*if (PORTB.RB2 == 0 && PORTB.RB0 == 0){
            LimparTela();
            }*/
         segundo = segundo + 1;
         MostrarSegundo(segundo);


         }
 }
