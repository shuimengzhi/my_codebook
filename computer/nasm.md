[TOC]
# disassembler 
 ndisasmw -o 0x7c00 boot.bin >> disboot.asm
 0x7c00是反编译起始地址，不然找不到地方
# what's mean about $?
```
msg:   db "Enter a digit "
msgend: 
Length equ msgend - msg
Length2 equ $ - msg     ; Length2 = Length
```
current address
$ is that this line start address
# when should I use "%"?
- use registers like:%eax
# 3.2 Pseudo-Instructions
## 3.2.1 Dx: Declaring Initialized Data
```
      db    0x55                ; just the byte 0x55 
      db    0x55,0x56,0x57      ; three bytes in succession 
      db    'a',0x55            ; character constants are OK 
      db    'hello',13,10,'$'   ; so are string constants 
      dw    0x1234              ; 0x34 0x12 
      dw    'a'                 ; 0x61 0x00 (it's just a number) 
      dw    'ab'                ; 0x61 0x62 (character constant) 
      dw    'abc'               ; 0x61 0x62 0x63 0x00 (string) 
      dd    0x12345678          ; 0x78 0x56 0x34 0x12 
      dd    1.234567e20         ; floating-point constant 
      dq    0x123456789abcdef0  ; eight byte constant 
      dq    1.234567e20         ; double-precision float 
      dt    1.234567e20         ; extended-precision float
```
Note that a list needs to be prefixed with a % sign unless prefixed by either DUP or a type in order to avoid confusing it with a parentesis starting an expression. The following expressions are all valid:
```
       db 33 
       db (44)               ; Integer expression 
     ; db (44,55)            ; Invalid - error 
       db %(44,55) 
       db %('XX','YY') 
       db ('AA')             ; Integer expression - outputs single byte 
       db %('BB')            ; List, containing a string 
       db ? 
       db 6 dup (33) 
       db 6 dup (33, 34) 
       db 6 dup (33, 34), 35 
       db 7 dup (99) 
       db 7 dup dword (?, word ?, ?) 
       dw byte (?,44) 
       dw 3 dup (0xcc, 4 dup byte ('PQR'), ?), 0xabcd 
       dd 16 dup (0xaaaa, ?, 0xbbbbbb) 
       dd 64 dup (?)
```
## 3.2.4 EQU: Defining Constants
EQU defines a symbol to a given constant value: when EQU is used, the source line must contain a label. The action of EQU is to define the given label name to the value of its (only) operand. This definition is absolute, and cannot change later. So, for example,

```
message         db      'hello, world' 
msglen          equ     $-message
```
## 3.2.5 TIMES: Repeating Instructions or Data
The TIMES prefix causes the instruction to be assembled multiple times. This is partly present as NASM's equivalent of the DUP syntax supported by MASM–compatible assemblers, in that you can code
```
zerobuf:        times 64 db 0
```
or similar things; but TIMES is more versatile than that. The argument to TIMES is not just a numeric constant, but a numeric expression, so you can do things like