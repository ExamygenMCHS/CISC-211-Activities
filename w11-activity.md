## Task: Generate English uppercase characters from A to Z. After every character, there must be a line feed. Use procedures and loops to optimize the code. Do not use gdb debugger. The executable file will run directly on the terminal. See the output below.

<img src="https://user-images.githubusercontent.com/11669149/233437846-0d3dd8bb-fbc6-4f78-a6d6-29cf72d1e6a1.png" width=50%, height=50%>

**1.) Flowchart**

<img src="https://github.com/ExamygenMCHS/CISC-211-Activities/assets/70991350/b99a5d46-bd55-47da-b16f-f0dc90a2a37f" width=90%, height=90%>

**2.) What were your challenges in performing the lab (from design to the implementation phases)?**

My challenges in performing the lab were when I ran the code after working on it, I saw that only uppercase A was printed at first, and I was having trouble describing the code using comments. To overcome these challenges, I got help from Professor Khan and one of my classmates, and I used my past codes as references to help me describe my code for this activity.

**3.) The code**
```
section .text
        global _start           ;must be declared for linker (ld)
                                ;global is used to export the _start label
                                ;This will be the entry point to the program

_start:                         ;tells linker entry point
        mov eax,65              ;assigns eax to 65, which is the ASCII decimal for 
                                ;uppercase a (A)

loop:                           ;tells loop entry point
        mov [res],eax           ;assigns res to eax
        push eax                ;saves 65
        call output             ;calls output loop function
        pop eax                 ;restores saved number from push
        inc eax                 ;increments eax
        cmp eax,91              ;compares eax to 91, which is the ASCII decimal for
                                ;the left square bracket ([)
        jl loop                 ;jump to loop if number is less than 91
        call exit               ;calls exit loop function

output:                         ;tells output entry point
        mov edx,1               ;output length
        mov ebx,1               ;stdout
        mov ecx,res             ;assigns ecx to res
        mov eax,4               ;system call (sys_write)
        int 0x80                ;call kernel (interrupt 0x80)

        mov edx, 1              ;assigns edx register to 1
        mov ebx, 1              ;assigns ebx register  to 1
        mov ecx, space          ;assigns ecx register to space
        mov eax, 4              ;assigns eax register to 4
        int 0x80                ;call kernel (interrupt 0x80)
        ret                     ;return loop

exit:                           ;tells exit point
        mov eax,1               ;set eax register to 1
        int 0x80                ;call kernel (interrupt 0x80)

segment .bss
        res resb 1              ;uninitialized variable res (reserve a byte)

section .data
        space dd 10             ;initialized variable space
```
