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
```
IN PROGRESS
```
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
IN PROGRESS
```

**3.) Output.txt file**
```
IN PROGRESS
```
**4.) Video explaining the code (MM:SS):** https://www.youtube.com/watch?v=[VideoID]
