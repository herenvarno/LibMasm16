; Standard library for masm assembly language programming
; By Edward Yang (herenvarno@mail.com)
; Copyright MIT License

;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
; FUNC: clrscr
; ARGS: void
; RETS: void
; DESC: clear screen
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
clrscr	proc
		push	ax
		push	bx
		push	cx
		push	dx
		mov		ax, 0600h		; 06 : to scroll upward
								; 00 : lines=0 means for clear full screen
		mov		bh, 0fh			; 0 : for background, black
								; f : for foreground, white
		mov		cx, 0000h		; starting coordinates
		mov		dx, 184fh		; ending coordinates
		int		10h				; interrupt for executing video display
		mov		ah, 4ch			; return to dos mode
		int		21h				; interrupt for return to dos
		pop		dx
		pop		cx
		pop		bx
		pop		ax
		ret
clrscr	endp

;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
; FUNC: crlf
; ARGS: void
; RETS: void
; DESC: make cursor to the start of a new line
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
crlf	proc
		push	ax
		push	dx
		mov		ah, 02h			; configure 21h interrupt to be "print a character to std output"
		mov		dl, 0dh			; Carry Return <CR>
		int		21h				; interrupt
		mov		dl, 0ah			; Line Feed <LF>
		int		21h				; interrupt
		pop		dx
		pop		ax
		ret
crlf	endp

;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
; FUNC: delay UNDO
; ARGS: al -- the time to delay, unit is second
; RETS: void
; DESC: delay a certain time
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
delay	proc
		push	ax
		push	bx
		push	cx
		push	dx
		mov		cx, 000fh		; cx:dx = the ms to delay by a 15h interrupt
		mov		dx, 4240h
		mov		ah, 86h			; configure the 15h interrupt to be "wait"
delay_lop:
		int		15h				; interrupt
		sub		al, 1
		cmp		al, 0
;		jg		delay_lop
		pop		dx
		pop		cx
		pop		bx
		pop		ax
delay	endp

;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
; FUNC:	dump_mem
; ARGS:	si -- the starting address
;		cx -- the number of units
;		bx -- the unit size (1 for B, 2 for W, 4 for DW)
; RETS:	void
; DESC:	write a range of memory to std output in HEX
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
dump_mem	proc
		push	ax
		push	bx
		push	cx
		push	dx
		push	si
		mov		ah, 02h			; configure 21h for std output
dump_mem_lop1:
		push	cx				; push cx for further use
		mov		cx, bx			; use cx as a counter for inner loop
		push	bx				; push bx for further use
		sub		bx, 1			; use bx as the index pointer offset pointer should be smaller than counter
dump_mem_lop2:
		mov		dl, [si+bx]		; load the byte data
		call	write_hex		; print in HEX format
		dec		bx				; decrease bx for higher part
		loop	dump_mem_lop2
		pop		bx
		pop		cx
		mov		dl, 'H'			; print 'H' for indicate HEX format
		int		21h
		mov		dl, ' '			; print space for division
		int		21h
		add		si, bx
		loop	dump_mem_lop1
		pop		si
		pop		dx
		pop		cx
		pop		bx
		pop		ax
		ret
dump_mem	endp

;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
; FUNC: dum_regs
; ARGS: void
; RETS: void
; DESC: print all registers into std output
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
dump_regs	proc
		push	ax
		push	bx
		push	cx
		push	dx
		pushf
		
		; for ax
		mov		dl, 'A'
		call	write_char
		mov		dl, 'X'
		call	write_char
		mov		dl, '='
		call	write_char
		mov		dl, ah
		call	write_hex
		mov		dl, al
		call	write_hex
		mov		dl, ' '
		call	write_char
		
		;for bx
		mov		dl, 'B'
		call	write_char
		mov		dl, 'X'
		call	write_char
		mov		dl, '='
		call	write_char
		mov		dl, bh
		call	write_hex
		mov		dl, bl
		call	write_hex
		mov		dl, ' '
		call	write_char
		
		;for cx
		mov		dl, 'C'
		call	write_char
		mov		dl, 'X'
		call	write_char
		mov		dl, '='
		call	write_char
		mov		dl, ch
		call	write_hex
		mov		dl, cl
		call	write_hex
		mov		dl, ' '
		call	write_char
		
		;for dx
		mov		dl, 'D'
		call	write_char
		mov		dl, 'X'
		call	write_char
		mov		dl, '='
		call	write_char
		pop		ax				; flags
		pop		dx
		push	dx
		push	ax
		xchg	dl, dh
		call	write_hex
		xchg	dh, dl
		call	write_hex
		mov		dl, 0dh			; break line for a new line
		call	write_char
		mov		dl, 0ah
		call	write_char

		;for si
		mov		dl, 'S'
		call	write_char
		mov		dl, 'I'
		call	write_char
		mov		dl, '='
		call	write_char
		mov		dx, si
		xchg	dl, dh
		call	write_hex
		xchg	dh, dl
		call	write_hex
		mov		dl, ' '
		call	write_char

		;for di
		mov		dl, 'D'
		call	write_char
		mov		dl, 'I'
		call	write_char
		mov		dl, '='
		call	write_char
		mov		dx, di
		xchg	dl, dh
		call	write_hex
		xchg	dh, dl
		call	write_hex
		mov		dl, ' '
		call	write_char

		;for bp
		mov		dl, 'B'
		call	write_char
		mov		dl, 'P'
		call	write_char
		mov		dl, '='
		call	write_char
		mov		dx, bp
		xchg	dl, dh
		call	write_hex
		xchg	dh, dl
		call	write_hex
		mov		dl, ' '
		call	write_char

		;for sp
		mov		dl, 'S'
		call	write_char
		mov		dl, 'P'
		call	write_char
		mov		dl, '='
		call	write_char
		mov		dx, sp
		xchg	dl, dh
		call	write_hex
		xchg	dh, dl
		call	write_hex
		mov		dl, 0dh
		call	write_char
		mov		dl, 0ah
		call	write_char

		;for flags
		mov		dl, 'F'
		call	write_char
		mov		dl, 'L'
		call	write_char
		mov		dl, '='
		call	write_char
		pop		dx
		push	dx
		xchg	dl, dh
		call	write_hex
		xchg	dh, dl
		call	write_hex
		mov		dl, ' '
		call	write_char

		;for cf
		mov		dl, 'C'
		call	write_char
		mov		dl, 'F'
		call	write_char
		mov		dl, '='
		call	write_char
		pop		dx
		push	dx
		and		dx, 01h
		call	write_dec
		mov		dl, ' '
		call	write_char

		;for sf
		mov		dl, 'S'
		call	write_char
		mov		dl, 'F'
		call	write_char
		mov		dl, '='
		call	write_char
		pop		dx
		push	dx
		mov		cl, 7
		shr		dx, cl
		and		dx, 01h
		call	write_dec
		mov		dl, ' '
		call	write_char
		
		;for zf
		mov		dl, 'Z'
		call	write_char
		mov		dl, 'F'
		call	write_char
		mov		dl, '='
		call	write_char
		pop		dx
		push	dx
		mov		cl, 6
		shr		dx, cl
		and		dx, 01h
		call	write_dec
		mov		dl, ' '
		call	write_char

		;for of
		mov		dl, 'O'
		call	write_char
		mov		dl, 'F'
		call	write_char
		mov		dl, '='
		call	write_char
		pop		dx
		mov		cl, 11
		shr		dx, cl
		and		dx, 01h
		call	write_dec
		mov		dl, 0dh
		call	write_char
		mov		dl, 0ah
		call	write_char
		pop		dx
		pop		cx
		pop		bx
		pop		ax
		ret
dump_regs	endp

;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
; FUNC: write_hex
; ARGS: dl -- the byte to be print
; RETS: void
; DESC: print a byte in HEX format
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
write_hex	proc
		push	ax
		push	cx
		push	dx
		mov		ah, 2h
		mov		dh, dl
		mov		cl, 4
		shr		dl, cl
		and		dl, 0fh
		cmp		dl, 0ah
		jl		write_hex_add0
		sub		dl, 0ah
		add		dl, 'A'
		jmp		write_hex_over0
write_hex_add0:
		add		dl, '0'
write_hex_over0:
		int		21h
		mov		cl, 8
		shr		dx, cl
		and		dl, 0fh
		cmp		dl, 0ah
		jl		write_hex_add1
		sub		dl, 0ah
		add		dl, 'A'
		jmp		write_hex_over1
write_hex_add1:
		add		dl, '0'
write_hex_over1:
		int		21h
		pop		dx
		pop		cx
		pop		ax
		ret
write_hex	endp

;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
; FUNC: write_dec
; ARGS: dl -- the byte to be print
; RETS: void
; DESC: print a byte in DEC format
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
write_dec	proc
		push	ax
		push	bx
		push	cx
		push	dx
		xor		ax,	ax
		xor		cx, cx
		mov		al, dl
		mov		bl, 10
write_dec_lop1:
		div		bl
		mov		dl, ah
		add		dl, '0'
		push	dx
		inc		cx
		mov		ah, 0
		cmp		al, 10
		jnl		write_dec_lop1
		mov		ah, 2h
		cmp		al, 0
		je		write_dec_lop2
		mov		dl, al
		add		dl, '0'
		int		21h
write_dec_lop2:
		pop		dx
		int		21h
		loop	write_dec_lop2
		pop		dx
		pop		cx
		pop		bx
		pop		ax
		ret
write_dec	endp

;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
; FUNC: write_char
; ARGS: dl -- the char to be print
; RETS: void
; DESC: print a char to std output
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
write_char	proc
		push	ax
		mov		ah, 2h
		int		21h
		pop		ax
		ret
write_char	endp

;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
; FUNC: write_int
; ARGS: dl -- the int to be print
; RETS: void
; DESC: print a interger to std output
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
write_int	proc
		push	ax
		push	dx
		cmp		dl, 0
		jns		write_int_ctn
		push	dx
		mov		ah, 2h
		mov		dl, '-'
		int		21h
		pop		dx
		neg		dl
write_int_ctn:
		call	write_dec
		pop		dx
		pop		ax
		ret
write_int	endp

