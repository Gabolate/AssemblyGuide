# AssemblyGuide
Guide for programming in Assembly. More stuff added as i learn.


# General Commands

- Setting values

  It can be used to set variables

  Usage:
  ```asm
  mov destiny, source
  ```

  Example:

  ```asm
  mov ax, 0 ; this sets ax with 0
  mov ax, 0 + 1 ; this sets ax with 1 (0 + 1)
  mov ax, bx ; this sets ax with the data stored in bx
  ```

- Jump

  Jumps to a label, but code after it gets "lost" (basically, it dosen't get executed)

  Usage:
  ```asm
  jmp labelName
  ```

  Example:

  ```asm
  jmp mylabel ; Jumps to mylabel
  mov ax, 0 ; this won't be executed since the computer is running stuff from mylabel's code and onwards

  mylabel:
  mov ax, bx ; sets ax with bx
  ; code after this will be executed
  ```

- Labels

  These can be used to create points in code that can be accessed later.

  Usage :
  ```asm
  labelName:
  ```
  
  Example:

  ```asm
  jmp mylabel ; jump to mylabel
  
  ; more code
  
  mylabel: ; the label
  mov ax, 0 ; sets ax to 0
  ```

- Calls

  Jumps to a label but after the function/label ended execution it returns to where it was originally

  Usage:
  ```asm
  call labelname
  ```
  
  Example:

  ```asm
  call mylabel ; call mylabel
  mov ax, bx ; unlike jmp, this code WILL be executed after mylabel returns
  
  ; more code
  
  mylabel: ; the label
  mov ax, 0 ; sets ax to 0
  ret ; return
  ```

- Return

  Exits an ongoing label/function.
  
  Usage:
  ```asm
  ret
  ```
  
  Example:

  ```asm

  call mylabel ; calls mylabel
  ; after 'ret' it continues execution here
  mov bx, ax ; set bx with ax

  ; more code
  
  mylabel: ; the label or function

  mov ax, 0 ; sets ax to 0
  ret ; returns
  ```

- Define Byte

  Used to define 1 or multiple bytes
  
  Usages:
  ```asm
  db value
  db value1, value2
  mybyte db value
  ```

  Example:

  ```asm
  mytext db "Hello World!", 0 ; 'Hello World!' text with a NULL byte at the end. its memory address can be found by accessing 'mytext'
  ```

- Define Word

  Used to define 1 or multiple words (2 bytes, equivalent to a 'short' type in C# or a 16-Bit integer)

  Usages:
  ```asm
  dw value
  dw value1, value2
  wordname dw value
  ```

  Example:

  ```asm
  dw 0AA55h ; MBR signature in hexadecimal (bytes 55 and AA)
  dw 0FF02h, 0AA55h ; 2 bytes and the MBR signature (first 02 and FF, finally 55 and AA)
  ```
  
