# DA1B

; Assignment1B.asm
; Created: 2/23/2019 1:33:36 PM
; Author : Biruk Gebremeskel
; Instructor : Dr. Venki Muthukumar


RESET:
.equ STARTADDS = 0x0200		; starting adress
.equ count = 109			;  the counter to 109


.org 0
	clr r0		; clearing r0 resister
	ldi XL, low(STARTADDS)		; XL = STARTADDS[7:0]
	ldi XH, high(STARTADDS)		; XH = STARTADDS[15:8]
	ldi YL, low(0x400)		; YL = 0x00
	ldi YH, high(0x400)		; YH = 0x04
	ldi ZL, low(0x600)		; ZL = 0x00
	ldi ZH, high(0x600)		; ZH = 0x06

	; initializing count registers
	ldi r21, count	; r21 = 109
	ldi r22, 10	; r22 = 10
	ldi r20, 3		; r20 = 3

	; Clearing Registers for sums
	clr r0	; r0 = 0
	clr r16 ; r16 = 0
	clr r17 ; r17 = 0
	clr r18 ; r18 = 0
	clr r19	; r19 = 0
	clr r5	; r5 = 0
	inc r5	; r5 = 1

START:
	; Adding lower and upper address bits to be stored
	mov r1, XL	; R1 = XL
	add r1, XH	; R1 = XL + XH
	mov r3, r1	; R3 = R1
	st x+, r3	; [x] = XL + XH

	; Checking if divisible by 3
DIVBYTHREE:
	cp r1, r20	; if(R1 < 3)
	brlo DIVBAD	; go to DIVBAD
	sub r1, r20	; R1 = R1 - R20
	cp r1, r0	; if (R1 == 0)
	breq DIVGOOD	; go to DIVGOOD
	rjmp	DIVBYTHREE	; go to DIVBYTHREE

DIVGOOD:
	st Z+, r3	;[Z] = XL + XH
	add r16, r3	; R16 = R16 + R3 (lower)
	adc r17, r0	; R17 = R17 + R0 + C (upper)
	rjmp DONE	; go to DONE

DIVBAD:
	st Y+, r1	; [Y] = XL + XH
	add r18, r3	; R18 = R18 + R3 (lower)
	adc r19, r0 ; R19 = R19 + R0 + C (upper)
	rjmp DONE	; go to DONE


DONE:
	cp r21, r0	; if(count == 0)
	breq NOTYET	; go to NOTYET
	sub r21, r5 ; R21 = R21 - R5
	brne START	; if(R21 != 0) go to START

NOTYET:
	sub r22, r5	; R22 = R22 - R5
	brne START	; if( R22 != 0) go to START

FIN:
	rjmp	RESET	; Reset

