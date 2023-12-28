list p=16f877a
#include<p16f877a.inc>

	org 0x00
	goto MAIN


	ORG 0x50 
MAPPING:
    ADDWF PCL,F      
    RETLW    0XC0
    RETLW    0xF9
    RETLW    0xA4
    RETLW    0xB0
    RETLW    0x99
    RETLW    0x92
    RETLW    0x82
    RETLW    0xF8
    RETLW    0x80
    RETLW    0x90

	ORG	0X500
MAIN:
	BSF STATUS, RP0      
    MOVLW b'00111000'   
    MOVWF OPTION_REG
   	movlw	b'11111111'
    MOVWF TRISA
	movlw b'00000000'
	movwf TRISB
	movwf TRISD
    BCF STATUS, RP0  
	movwf PORTD
	movwf PORTB
	movlw 0x00
	movwf 0x22  	 
CLEART:	 
	movlw 0x00         
    MOVWF TMR0
	movwf 0x21

	
Loop:
	movf TMR0,W
	movwf 0x21
	call DISPLAY
	bcf STATUS,Z
	movlw b'01100100'
	subwf TMR0,W
	BTFSC STATUS,Z
	goto CLEART
	GOTO	Loop

DISPLAY:
	movlw b'00001010'
	subwf 0x21,F
	movlw b'00000001'
	btfss 0x21,7
	addwf 0x22,F
	btfss 0x21,7
	goto DISPLAY
	movlw b'00001010'
	addwf 0x21,F
	movf  0x21,W
	call  MAPPING
	movwf PORTB
	movf  0x22,W
	call  MAPPING	
	movwf PORTD
	movlw 0x00 
	movwf 0x22
	RETURN


	END			
