## Task 1: Create a text-based file called "quotes.txt" and add the following contents: (4 marks)
"To be, or not to be, that is the question."

"A fool thinks himself to be wise, but a wise man knows himself to be a fool."

## Task 2: Append the following quotes in the same file. (5 marks)
"Better three hours too soon than a minute too late."

"No legacy is so rich as honesty."


**1.) What were your challenges in performing the lab (from design to the implementation phases)? (1 mark)**

My challenge in performing the lab was that when I was working on the second task, which is when I append quotes into quotes.txt, the appending didn't work at first. So, I did both the tasks in three separate .asm files, and I did the first task in two separate .asm files: one for creating the text file, and one for adding the first quotes into the file. For the second task, this is where I'm going to append the last two quotes into the text file. While I was running my .asm file for the second task for the first time, I realized that the appending didn't work. To solve this problem, I was thinking of adding the write file operation to make the appending work.

**2.) Codes (3 separate codes)**

a.) Create a file (Task 1)
```
SECTION .text
        global  _start                  ;must be declared for linker (ld)
                                        ;global is used to export the _start label
                                        ;This will be the entry point to the program

_start:                                 ;tells linker entry point
        mov ecx, 0711o                  ;set all permissions to read, write, execute (octal format)
        mov ebx, filename               ;this will create a file name for the text file
        mov eax, 8                      ;invoke SYS_CREAT (kernel opcode 8)
        int 0x80                        ;call kernel

        mov eax,1                       ;set eax register to 1
        int 0x80                        ;call kernel

SECTION .data
        filename db 'quotes.txt', 0h    ;the filename to create
```

b.) Add the first two quotes into a text file (Task 1)
```
SECTION .data
        filename db 'quotes.txt', 0h                                                                                                                                    ;this is the desired text file
                                                                                                                                                                        ;to modify by adding contents
                                                                                                                                                                        ;into it
        contents1 db 'To be, or not to be, that is the question.', 0xa, 'A fool thinks himself to be wise, but a wise man knows himself to be a fool.', 0xa             ;these are the desired contents
                                                                                                                                                                        ;to add into the desired text file
        len1 equ $ - contents1                                                                                                                                          ;the total length of the quotes
                                                                                                                                                                        ;from contents1

SECTION .text
        global _start                                                                                                                                                   ;must be declared for linker (ld)
                                                                                                                                                                        ;global is used to export the _start label. 
                                                                                                                                                                        ;This will be the entry point to the program.

_start:                                                                                                                                                                 ;tells linker entry point
        ;open file operation
        mov eax, 5                                                                                                                                                      ;file-handling system call to open a file (sys_open)
        mov ebx, filename                                                                                                                                               ;filename we will create
        mov ecx, 1                                                                                                                                                      ;file access mode
        mov edx, 0777o                                                                                                                                                  ;file permissions
        int 0x80                                                                                                                                                        ;call kernel

        mov [fd_out], eax                                                                                                                                               ;assigns file descriptor to eax

        ;write file operation
        mov eax, 4                                                                                                                                                      ;file-handling system call to write a file (sys_write)
        mov ebx, [fd_out]                                                                                                                                               ;file descriptor
        mov ecx, contents1                                                                                                                                              ;this will write the contents to the file
        mov edx, len1                                                                                                                                                   ;length of the contents serves as the number of bytes written
        int 0x80                                                                                                                                                        ;call kernel

        ;close file operation
        mov eax, 6                                                                                                                                                      ;file-handling system call to close a file (sys_close)
        mov ebx, [fd_out]                                                                                                                                               ;file descriptor
        int 0x80                                                                                                                                                        ;call kernel

        mov eax, 1                                                                                                                                                      ;set eax register to 1
        int 0x80                                                                                                                                                        ;call kernel

section .bss
        fd_out resb 1                                                                                                                                                   ;file descriptor serves as the uninitialized variable (reserve
                                                                                                                                                                        ;a byte)
```

c.) Append the last two quotes into a text file (Task 2)
```
SECTION .data
        filename db 'quotes.txt', 0h                                                                                                    ;this is the desired text file
                                                                                                                                        ;to modify by adding
                                                                                                                                        ;and appending contents into it
        contents2 db 'Better three hours too soon than a minute too late.', 0xa, 'No legacy is so rich as honesty.'                     ;these are the desired contents
                                                                                                                                        ;to append and add into the desired
                                                                                                                                        ;text file
        len2 equ $ - contents2                                                                                                          ;the total length of quotes from
                                                                                                                                        ;contents2

SECTION .text
        global _start                                                                                                                   ;must be declared for linker (ld)
                                                                                                                                        ;global is used to export the _start label
                                                                                                                                        ;This will be the entry point to the program

_start:                                                                                                                                 ;tells linker entry point
        ;open file operation
        mov eax, 5                                                                                                                      ;file-handling system call to open a file (sys_open)
        mov ebx, filename                                                                                                               ;filename we will create
        mov ecx, 1                                                                                                                      ;file access mode
        mov edx, 0777o                                                                                                                  ;file permissions
        int 0x80                                                                                                                        ;call kernel

        mov [fd_out], eax                                                                                                               ;assigns file descriptor to eax

        ;append file operation
        ;This will update the text file by
        ;adding the last two quotes into
        ;the text file.
        mov eax, 19                                                                                                                     ;file-handling system call to append contents into
                                                                                                                                        ;file (sys_lseek)
        mov ebx, [fd_out]                                                                                                               ;file descriptor
        mov ecx, 0                                                                                                                      ;offset value
        mov edx, 2                                                                                                                      ;reference position for the offset (2 means end of the
                                                                                                                                        ;file)
        int 0x80                                                                                                                        ;call kernel

        ;write file operation
        ;In order to append the quotes, the code
        ;must have a write file operation.
        mov eax, 4                                                                                                                      ;file-handling system call to write a file (sys_write)
        mov ebx, [fd_out]                                                                                                               ;file descriptor
        mov ecx, contents2                                                                                                              ;this will write the contents to the file
        mov edx, len2                                                                                                                   ;length of the contents serves as the number of bytes written
        int 0x80                                                                                                                        ;call kernel

        ;close file operation
        mov eax, 6                                                                                                                      ;file-handling system call to close a file (sys_close)
        mov ebx, [fd_out]                                                                                                               ;file descriptor
        int 0x80                                                                                                                        ;call kernel

        mov eax, 1                                                                                                                      ;set eax register to 1
        int 0x80                                                                                                                        ;call kernel

section .bss
        fd_out resb 1                                                                                                                   ;file descriptor serves as the uninitialized variable (reserve a byte)
```
