# Task I chose: Task #1 (Encryption and decryption)

## Task #1: Encrypt and decrypt using an XOR logical operator. (Due May 18th @ 11:59pm)

### Encrypting a message:

- Select a secret "one English word" message. Keep it secret.

- Create a secret “one English word” key. For simplicity, use the same length of the word key as the message. For example, if the message is "Hello," select a key size equal to 5 characters, such as "March."

- Apply XOR bitwise operation between the message and the key.

### Decrypting a message:

- Use the "encrypted message" and the "secret key" and perform the bitwise XOR operation.

- Verify that the decrypted message is the same as the "plain text."

### Output to display

The output should be saved in a text file (output.txt). For example:
```
Plain text: Hello
Key: March
Encrypted text: 5041e0f07
Decrypted text: Hello
```


**1.) Flowchart**
![Project drawio (2)](https://github.com/ExamygenMCHS/CISC-211-Activities/assets/70991350/609034a2-531b-4154-8b8c-cfe9ed1b41b4)
**2.) Codes (Two separate codes)**

a.) Create a file Output.txt
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
        filename db 'Output.txt', 0h    ; the filename to create
```

b.) Main code for encrypting and decrypting a message:
```
SECTION .data
	filename db 'Output.txt', 0h			;this is the desired text file to modify

	newline dd 0xa					;this will make new lines

	;labels
	pt_label db 'Plain text: ', 0h			;label for plain text
	lenpt equ $ - pt_label				;length for the plain text label

	key_label db 'Key: ', 0h			;label for key
	lenkey equ $ - key_label			;length for the key label

	encrypted_label db 'Encrypted text: ', 0h	;label for encrypted text
	lenenc equ $ - encrypted_label			;length for the encrypted text label

	decrypted_label db 'Decrypted text: ', 0h	;label for decrypted text
	lendec equ $ - decrypted_label			;length for the decrypted text label

	;letters
	pt_letter1 db 'A'				;first letter of the plain text
	key_letter1 db 'r'				;first letter of the key

	pt_letter2 db 'U'				;second letter of the plain text
	key_letter2 db 'a'				;second letter of the key

	pt_letter3 db 'T'				;third letter of the plain text
	key_letter3 db 'c'				;third letter of the key

	pt_letter4 db 'O'				;fourth letter of the plain text
	key_letter4 db 'e'				;fourth letter of the key

	;characters as results
	encrypted_result1 db 0h				;first character of the encrypted text
	encrypted_result2 db 0h				;second character of the encrypted text
	encrypted_result3 db 0h				;third character of the encrypted text
	encrypted_result4 db 0h				;fourth character of the encrypted text

	decrypted_result1 db 0h				;first character of the decrypted text
	decrypted_result2 db 0h				;second character of the decrypted text
	decrypted_result3 db 0h				;third character of the decrypted text
	decrypted_result4 db 0h				;fourth character of the decrypted text

SECTION .bss
	fd_out resb 1					;file descriptor serves as the uninitialized variable

SECTION .text
	global _start					;must be declared for linker (ld)
							;global is used to export the _start label
							;This will be the entry point to the program.

_start:							;tells linker entry point
	;encryption process
	;first character
	mov eax, [pt_letter1]				;assigns eax register to pt_letter1
	mov ebx, [key_letter1]				;assigns ebx register to key_letter1
	xor eax, ebx					;xor operation between eax and ebx
	mov [encrypted_result1], eax			;declares first result for eax

	;second character
	mov ecx, [pt_letter2]				;assigns ecx register to pt_letter2
	mov edx, [key_letter2]				;assigns edx register to key_letter2
	xor ecx, edx					;xor operation between ecx and edx
	mov [encrypted_result2], ecx			;declares second result for ecx

	;third character
	mov al, [pt_letter3]				;assigns al register to pt_letter3
	mov bl, [key_letter3]				;assigns edx register to key_letter3
	xor al, bl					;xor operation between al and bl
	mov [encrypted_result3], al			;declares third result for al

	;fourth character
	mov cl, [pt_letter4]				;assigns cl register to pt_letter4
	mov dl, [key_letter4]				;assigns dl register to key_letter4
	xor cl, dl					;xor operation between cl and dl
	mov [encrypted_result4], cl			;declares fourth result for cl

	;decryption process
	;first character
	mov ax, [encrypted_result1]			;assigns ax register to encrypted_result1
	mov bx, [key_letter1]				;assigns bx register to key_letter1
	xor ax, bx					;xor operation between ax and bx
	mov [decrypted_result1], ax			;declares first result for ax

	;second character
	mov cx, [encrypted_result2]			;assigns cx register to encrypted_result2
	mov dx, [key_letter2]				;assigns dx register to key_letter2
	xor cx, dx					;xor operation between cx and dx
	mov [decrypted_result2], cx			;declares second result for cx

	;third character
	mov ah, [encrypted_result3]			;assigns ah register to encrypted_result3
	mov bh, [key_letter3]				;assigns bh reigster to key_letter3
	xor ah, bh					;xor operation between ah and bh
	mov [decrypted_result3], ah			;declares third result for ah

	;fourth character
	mov ch, [encrypted_result4]			;assigns ch register to encrypted_result4
	mov dh, [key_letter4]				;assigns dh register to key_letter4
	xor ch, dh					;xor operation between ch and dh
	mov [decrypted_result4], ch			;declares fourth result for ch

	;open file operation
	mov eax, 5					;file-handling system call to open a file (sys_open)
	mov ebx, filename				;desired text file to modify
	mov ecx, 1					;file access mode
	mov edx, 0777o					;file permissions
	int 0x80					;call kernel

	mov [fd_out], eax				;this will print the output into the desired text file

	;write file operation
	mov eax, 4					;file-handling system call to write a file (sys_write)
	mov ebx, [fd_out]				;file descriptor
	mov ecx, pt_label				;this will write the plain text label to the file
	mov edx, lenpt					;number of bytes in the plain text label
	int 0x80					;call kernel

	;write file operation
	mov eax, 4					;file-handling system call to write a file (sys_write)
	mov ebx, [fd_out]				;file descriptor
	mov ecx, pt_letter1				;this will write the first letter of the plain text to the file
	mov edx, 1					;it's going to be a byte because it's just a letter
	int 0x80					;call kernel

	;write file operation
	mov eax, 4					;file-handling system call to write a file (sys_write)
        mov ebx, [fd_out]				;file descriptor
        mov ecx, pt_letter2				;this will write the second letter of the plain text to the file
        mov edx, 1					;it's going to be a byte because it's just a letter
        int 0x80					;call kernel

	;write file operation
	mov eax, 4					;file-handling system call to write a file (sys_write)
        mov ebx, [fd_out]				;file descriptor
        mov ecx, pt_letter3				;this will write the third letter of the plain text to the file
        mov edx, 1					;it's going to be a byte because it's just a letter
        int 0x80					;call kernel

	;write file operation
	mov eax, 4					;file-handling system call to write a file (sys_write)
        mov ebx, [fd_out]				;file descriptor
        mov ecx, pt_letter4				;this will write the fourth letter of the plain text to the file
        mov edx, 1					;it's going to be a byte because it's just a letter
        int 0x80					;call kernel

	;write file operation
	mov eax, 4					;file-handling system call to write a file (sys_write)
        mov ebx, [fd_out]				;file descriptor
        mov ecx, newline				;this will write a new line to the file
        mov edx, 1					;it's going to be a byte because it's just a new line
        int 0x80					;call kernel

	;write file operation
	mov eax, 4					;file-handling system call to write a file (sys_write)
        mov ebx, [fd_out]				;file descriptor
        mov ecx, key_label				;this will write the key text label to the file
        mov edx, lenkey					;number of bytes in the key text label
        int 0x80					;call kernel

	;write file operation
	mov eax, 4					;file-handling system call to write a file (sys_write)
        mov ebx, [fd_out]				;file descriptor
        mov ecx, key_letter1				;this will write the first letter of the key text to the file
        mov edx, 1					;it's going to be a byte because it's just a letter
        int 0x80					;call kernel

	;write file operation
	mov eax, 4					;file-handling system call to write a file (sys_write)
        mov ebx, [fd_out]				;file descriptor
        mov ecx, key_letter2				;this will write the second letter of the key text to the file
        mov edx, 1					;it's going to be a byte because it's just a letter
        int 0x80					;call kernel

	;write file operation
	mov eax, 4					;file-handling system call to write a file (sys_write)
        mov ebx, [fd_out]				;file descriptor
        mov ecx, key_letter3				;this will write the third letter of the key text to the file
        mov edx, 1					;it's going to be a byte because it's just a letter
        int 0x80					;call kernel

	;write file operation
	mov eax, 4					;file-handling system call to write a file (sys_write)
        mov ebx, [fd_out]				;file descriptor
        mov ecx, key_letter4				;this will write the fourth letter of the key text to the file
        mov edx, 1					;it's going to be a byte because it's just a letter
        int 0x80					;call kernel

	;write file operation
	mov eax, 4					;file-handling system call to write a file (sys_write)
        mov ebx, [fd_out]				;file descriptor
        mov ecx, newline				;this will write a new line to the file
        mov edx, 1					;it's going to be a byte because it's just a new line
        int 0x80					;call kernel

	;write file operation
	mov eax, 4					;file-handling system call to write a file (sys_write)
        mov ebx, [fd_out]				;file descriptor
        mov ecx, encrypted_label			;this will write the encrypted text label to the file
        mov edx, lenenc					;number of bytes in the encrypted text label
        int 0x80					;call kernel

	;append file operation
	mov eax, 19					;file-handling system call to append contents into
							    ;file (sys_lseek)
	mov ebx, [fd_out]				;file descriptor
	mov ecx, 45					;offset value
	mov edx, 0					;reference position
	int 0x80					;call kernel

	;write file operation
	mov eax, 4					;file-handling system call to write a file (sys_write)
        mov ebx, [fd_out]				;file descriptor
        mov ecx, encrypted_result1			;this will write the first character of the encrypted text to the file
        mov edx, 1					;it's going to be a byte because it's just a character
        int 0x80					;call kernel

	;write file operation
	mov eax, 4					;file-handling system call to write a file (sys_write)
        mov ebx, [fd_out]				;file descriptor
        mov ecx, encrypted_result2			;this will write the second character of the encrypted text to the file
        mov edx, 1					;it's going to be a byte because it's just a character
        int 0x80					;call kernel

	;write file operation
	mov eax, 4					;file-handling system call to write a file (sys_write)
        mov ebx, [fd_out]				;file descriptor
        mov ecx, encrypted_result3			;this will write the third character of the encrypted text to the file
        mov edx, 1					;it's going to be a byte because it's just a character
        int 0x80					;call kernel

	;write file operation
	mov eax, 4					;file-handling system call to write a file (sys_write)
        mov ebx, [fd_out]				;file descriptor
        mov ecx, encrypted_result4			;this will write the fourth character of the encrypted text to the file
        mov edx, 1					;it's going to be a byte because it's just a character
        int 0x80					;call kernel

	;write file operation
	mov eax, 4					;file-handling system call to write a file (sys_write)
        mov ebx, [fd_out]				;file descriptor
        mov ecx, newline				;this will write a new line to the file
        mov edx, 1					;it's going to be a byte because it's just a new line
        int 0x80					;call kernel

    ;write file operation
	mov eax, 4					;file-handling system call to write a file (sys_write)
        mov ebx, [fd_out]				;file descriptor
        mov ecx, decrypted_label			;this will write the decrypted text label to the file
        mov edx, lendec					;number of bytes in the decrypted text label
        int 0x80					;call kernel

	;write file operation
	mov eax, 4					;file-handling system call to write a file (sys_write)
        mov ebx, [fd_out]				;file descriptor
        mov ecx, decrypted_result1			;this will write the first character of the encrypted text to the file
        mov edx, 1					;it's going to be a byte because it's just a character
        int 0x80					;call kernel

	;write file operation
	mov eax, 4					;file-handling system call to write a file (sys_write)
        mov ebx, [fd_out]				;file descriptor
        mov ecx, decrypted_result2			;this will write the second character of the decrypted text to the file
        mov edx, 1					;it's going to be a byte because it's just a character
        int 0x80					;call kernel

	;write file operation
	mov eax, 4					;file-handling system call to write a file (sys_write)
        mov ebx, [fd_out]				;file descriptor
        mov ecx, decrypted_result3			;this will write the third character of the decrypted text to the file
        mov edx, 1					;it's going to be a byte because it's just a character
        int 0x80					;call kernel

	;write file operation
	mov eax, 4					;file-handling system call to write a file (sys_write)
        mov ebx, [fd_out]				;file descriptor
        mov ecx, decrypted_result4			;this will write the fourth character of the decrypted text to the file
        mov edx, 1					;it's going to be a byte because it's just a character
        int 0x80					;call kernel

	;close file operation
	mov eax, 6					;file-handling system call to close a file (sys_close)
	mov ebx, [fd_out]				;file descriptor
	int 0x80					;call kernel

	mov eax, 1					;set eax register to 1
	int 0x80					;call kernel
```

**3.) Output.txt file**
```
Plain text: AUTO
Key: race
Encrypted text: 347*
Decrypted text: AUTO
```
**4.) A YouTube video explaining the code (MM:SS):** https://www.youtube.com/watch?v=[VideoID]
