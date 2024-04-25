## Task 1: Create a text-based file called "quotes.txt" and add the following contents: (4 marks)
"To be, or not to be, that is the question."

"A fool thinks himself to be wise, but a wise man knows himself to be a fool."

## Task 2: Append the following quotes in the same file. (5 marks)
"Better three hours too soon than a minute too late."

"No legacy is so rich as honesty."

**1.) What were your challenges in performing the lab (from design to the implementation phases)? (1 mark)**
My challenges in performing the lab were...

**2.) Codes (3 separate codes)**

a.) Create a file (Task 1)
```
SECTION .text
        global  _start
 
_start:
 
        mov     ecx, 0711o ; set all permissions to read, write, execute (octal format)
        mov     ebx, filename       ; filename we will create
        mov     eax, 8              ; invoke SYS_CREAT (kernel opcode 8)
        int     0x80                ; call the kernel

        mov     eax,1
        int     0x80

SECTION .data
filename db 'quotes.txt', 0h    ; the filename to create
```

b.) Add the first two quotes into a text file (Task 1)
```
SECTION .data
        filename db 'quotes.txt', 0h
        contents1 db 'To be, or not to be, that is the question.', 0xa, 'A fool thinks himself to be wise, but a wise man knows himself to be a fool.', 0xa
        len1 equ $ - contents1

SECTION .text
        global _start

_start:
        ;open file operation
        mov eax, 5
        mov ebx, filename
        mov ecx, 1
        mov edx, 0777o
        int 0x80

        mov [fd_out], eax

        ;write file operation
        mov eax, 4
        mov ebx, [fd_out]
        mov ecx, contents1
        mov edx, len1
        int 0x80

        mov eax, 1
        int 0x80

section .bss
        fd_out resb 1
```

c.) Append the last two quotes into a text file (Task 2)
```
IN PROGRESS
```
