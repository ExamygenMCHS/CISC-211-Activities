## Task: Assign a number to a register or a variable, pass a number to the function and display the result, 'odd' or 'even'. The result should be displayed on the console. The function should check whether the number is an even or an odd number.

**1.) Flowchart**

**2.) What were your challenges in performing the lab (from design to the implementation phases)?**

**3.) The code**
```
section .text
        global _start                   ;must be declared for linker (ld)
                                        ;global is used to export the _start label
                                        ;this will be the entry point to the program

_start:                                 ;tells linker entry point
        push 10                         ;this will push the number into stack
        call _foobar                    ;call function for foobar
        jnz odd                         ;jump to odd if the number is not equal to 0
                                        ;(jump to odd if the number is odd)

        mov eax, 'E'                    ;returns as 69 in the console if the number is even
        mov [result], eax               ;prints 69 if the number is even
        mov eax, 1                      ;set eax register to 1
        int 0x80                        ;call kernel

odd:                                    ;tells odd entry point (this loop will be ignored
                                        ;if the number is even)
        mov eax, 'O'                    ;returns as 79 in the console if the number is odd
        mov [result], eax               ;prints 79 if the number is odd
        mov eax, 1                      ;set eax register to 1
        int 0x80                        ;call kernel

_foobar:                                ;tells foobar entry point
        push ebp                        ;this will save the ebp register into stack
        mov ebp, esp                    ;moves ebp register to esp register
        sub esp, 8                      ;subtracts esp register to 8 to make empty stacks

        mov eax, DWORD[ebp+8]           ;assigns eax register to ebp
        and eax, 1                      ;this examines if the number is odd or even
        mov DWORD[ebp-4], eax           ;this stores eax into ebp
        leave                           ;leave foobar
        ret                             ;return

segment .bss
        result resb 1                   ;uninitialized variable result (reserve a byte)
```
