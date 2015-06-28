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

  //Visão do Display
  //----------------//
  //VLT: 99.99-99.99//
  //TEMPO : 99 . 99 //
  //----------------//
  Lcd_Init();                        // Initialize LCD
  Lcd_Cmd(_LCD_CLEAR);               // Clear display
  Lcd_Cmd(_LCD_CURSOR_OFF);          // Cursor off
  Lcd_Out(1,1,"VLT: ");              // Write text in first row
  Lcd_Out(2,1,"TEMPO: ");            // Write text in first row

}


int pausar = 0;
int segundo = 0;
int mili = 0;
int mili1, segundo1, mili2, segundo2;

void interrupt_1(void){

 if (PORTB.RB1 == 0 || PORTB.RB2 == 0){

         while(pausar < 1){

         if (PORTB.RB2 == 0){
            pausar = 2;
            INTCON.INTF = 0;                   // CLEAR THE INTERRUPT FLAG
             }
             INTCON.INTF = 0;                      // CLEAR THE INTERRUPT FLAG
             }
            }

      pausar = 0;
}
char mili1_txt[2];
void mostrarTempos(){

   WordToStr(mili1,mili1_txt);
    Lcd_out(1,7,mili1_txt);
}
void interrupt(void) {                      // INTERRUPT SERVICE ROUTINE START


        interrupt_1();

            
      if (PORTB.RB1 == 0){
           mili1 = mili;
           segundo1 = segundo;
           mili2 = mili1;
           segundo2 = segundo1;
           
           segundo = 0;
           }

}


char segundo_txt[2];
void MostrarSegundo(int seg){

    WordToStr(seg,segundo_txt);
    Lcd_out(2,7,segundo_txt);

}

char miliSegundo_txt[2];
void MostrarMilisegundos(int milisegundo){

           WordToStr(milisegundo,miliSegundo_txt);
           Lcd_out(2,12,miliSegundo_txt);
           Lcd_Out(2,13,".");
           milisegundo++;

}

void esperaMilisegundo(){


         //Timer1 Registers Prescaler= 8 - TMR1 Preset = 40536 - Freq = 10.00 Hz - Period = 0.100000 seconds
         T1CON.T1CKPS1 = 1;   // bits 5-4  Prescaler Rate Select bits
         T1CON.T1CKPS0 = 1;   // bit 4
         T1CON.T1OSCEN = 1;   // bit 3 Timer1 Oscillator Enable Control bit 1 = on
         T1CON.T1SYNC = 1;    // bit 2 Timer1 External Clock Input Synchronization Control bit...1 = Do not synchronize external clock input
         T1CON.TMR1CS = 0;    // bit 1 Timer1 Clock Source Select bit...0 = Internal clock (FOSC/4)
         T1CON.TMR1ON = 1;    // bit 0 enables timer
         TMR1H = 158;         // preset for timer1 MSB register
         TMR1L = 88;          // preset for timer1 LSB register
         mili++;
         if (mili >= 100) {
            mili = 0;
            segundo++;
         }
         MostrarMilisegundos(mili);
      }
      
int cont = 0;
void main(){
   
   TRISB = 0b11111111;
   PORTB = 0b11111111;

   ADCON1 = 0x07;   //0x8E or 0x07
   LimparTela();
   
   INTCON.GIE = 1;                           // Habilita os interruptores
   INTCON.INTE = 1;                          // Habilita o RB0 como interruptor externo
   INTCON.PEIE = 0;                          // Disabilita os interruptores externos
   OPTION_REG.INTEDG = 0;                    // Interrupção "FALLING" EDGE

    while(1){

          // Ao apertar RB1 Limpa tela e Reinicia o ~Cornometro~
          if (PORTB.RB1 == 0){
           LimparTela();
           segundo = 0;
           delay_ms(1000);
           }


         esperaMilisegundo();
         cont++;

         if (cont == 100){
            MostrarSegundo(segundo);
            cont = 0;
            mostrarTempos();
            }

         }
 }
