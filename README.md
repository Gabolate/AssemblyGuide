# AssemblyGuide
Guide for programming in Assembly. More stuff added as i learn.


# General Commands

- ### Setting values

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

- ### Jump

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

- ### Labels

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

- ### Calls

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

- ### Return

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

- ### Define Byte

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

- ### Define Word

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

- ### Define Doubleword

  Used to define 1 or multiple doublewords (4 bytes, equivalent to a 'int' type in C# or a 32-Bit integer)

  Usages:
  ```asm
  dd value
  dd value1, value2
  ddname dd value
  ```

  Example:

  ```asm
  dd 100000 ; 32-bit integer, in 16-bit this number would not be possible 
  ```

- ### Define Quadword

  Used to define 1 or multiple quadwords (8 bytes, equivalent to a 'long' type in C# or a 64-Bit integer)

  Usages:
  ```asm
  dq value
  dq value1, value2
  dqname dq value
  ```

  Example:

  ```asm
  dq 10345879094 ; 64-bit integer, in 32-bit it wouldn't be possible
  ```

- ### Define Ten Bytes

  Used to define sets of 10 bytes

  Usages:
  ```asm
  dt value
  dt value1, value 2
  dtname dt value
  ```

  Example:

  ```asm
  hello dt "HelloWorld" ; "HelloWorld" text. its 10 ASCII characters (10 bytes) which means it can be used with 'dt'
  ```
  
- ### Times

  Used to fill data a certain amount of times

  Usages:
  ```asm
  times number db data
  variableName times number db data
  ```

  Example:

  ```asm
  myArray times 10 db 0 ; Creates an empty "array/variable" called myArray with a size of 10 bytes
  times 510 - ($ - $$) db 0 ; Fills with zeroes the program until it reaches 510 (used for MBR/bootloaders)
  ```

- ### Increment

  Increments 1 from a variable (Equivalent in C# to variable++;)

  Usage:
  ```asm
  inc variable
  ```

  Example:

  ```asm
  aNumber db 14 ; Declare a byte with a value of 14
  inc aNumber ; Increments 1 (14 + 1 = 15)
  ```

- ### Decrement

  Decrements 1 from a variable (Equivalent in C# to variable--;)

  Usage:
  ```asm
  dec variable
  ```

  Example:

  ```asm
  aNumber db 15 ; Declare a byte with a value of 15
  dec aNumber ; Decrements 1 (15 - 1 = 14)
  ```

- ### Addition

  Basic Addition function (Equivalent in C# to variable += number;)

  Usages:
  ```asm
  add destination, source
  add destination, number
  ```

  Example:

  ```asm
  firstNumber db 1 ; Declare a byte with a value of 1
  secondNumber db 2 ; Declare a byte with a value of 2
  add firstNumber, secondNumber ; Add secondNumber into firstNumber (2 + 1 = 3)
  ```

- ### Subtraction

  Basic Subtraction function (Equivalent in C# to variable -= number;)

  Usages:
  ```asm
  sub destination, source
  sub destination, number
  ```

  Example:
  
  ```asm
  firstNumber db 1 ; Declare a byte with a value of 1
  secondNumber db 2 ; Declare a byte with a value of 2
  sub firstNumber, secondNumber ; Subtract secondNumber into firstNumber (2 - 1 = 1)
  ```

- ### BIOS Interrupts

  Used to send an interrupt

  Usage:
  ```asm
  int value
  ```

  Example:

  ```asm
  int 13h ; BIOS Interrupt used for disk access
  ```
  
- ### Comparison

  Used to compare 2 values to later use a conditional jump.

  Usage:
  ```asm
  cmp firstValue, secondValue
  ```

  Example:

  ```asm
  counter db 0 ; Create a counter and start from 0
  
  loop: ; The test loop
  inc counter ; Increment the counter by 1
  cmp counter, 10 ; Compare the counter with 10
  jle loop ; Jump to 'loop' if the first value (counter) is less or equal to the second value (10) from the last comparison
  ```


- ### Conditional Jumps

  Here is a list of jump commands that can be used with comparison (``cmp``)

  Usage:
  ```asm
  cmp firstValue, secondValue ; First make a comparison between two values
  ; Then use one of the commands from the list below
  ```
  
  |Name for signed data|Name for UNsigned data|Description|
  |----|-----------|------|
  |``je`` or ``jz``|``je`` or ``jz``|Jumps if **both values** are *equal*|
  |``jne`` or ``jnz``|``jne`` or ``jnz``|Jumps if **both values** are *NOT equal*|
  |``jg`` or ``jnle``|``ja`` or ``jnbe``|Jump if the **first value** is *greater than* the **second value**|
  |``jge`` or ``jnl``|``jae`` or ``jnb``|Jump if the **first value** is *greater or equal than* the **second value**|
  |``jl`` or ``jnge``|``jb`` or ``jnae``|Jump if the **first value** is *less than* the **second value**|
  |``jle`` or ``jng``|``jbe`` or ``jna``|Jump if the **first value** is *less or equal than* the **second value**|

# Extra Details

- ### High and Lower Bytes on Registers

  In assembly we have something called Registers (e.g. AX, BX, CX, etc)

  which are usually used for calculation and when using Interrupts (On Interrupts its like the equivalent to Arguments)

  Registers on 16-bit mode (Real mode) are made of 2 bytes (Word).
  
  Registers' words can be accessed when the register ends up in 'X' (e.g. AX)

  You can access the first and second byte of a register by using 'H' for the higher (first) byte and 'L' for the lower (second) byte

  (e.g. AH and AL)
  

- ### Pointers

   Brackets ( ``[`` & ``]`` ) are used for pointers (equivalent to ``*`` in C# and ``&`` in C)

   Example:
  
   ```asm
   [testbyte]
   ```

# Interrupts Usage
  
  When using BIOS Interrupts you'll need some information stored in registers before calling the interrupt.

  (Some interrupts store result values)

  Take a look at the data tables to know what you need to specify for an specific interrupt.
  
  *Note: this section is not complete*

- ### INT 10h (Graphics)

  - ### AH 00h (Enable Video Mode)

    Main interrupt:

    |Register|Data|
    |--------|----|
    |AH|00h|
    |AL|Video mode|

    Returns:

    |Register|Data|
    |--------|----|
    |AL|Video mode flag (30h if mode is 0-5 and 7 | 3Fh if mode = 6 | 20h if mode is > 7)|

  - ### AH 01h (Set Text Mode Cursor Shape)
 
    Note:
    *This function changes the shape of text mode cursor.*

    *Each character usually uses 8 scan lines.*
 
    *CX = 0607h is a regular cursor*
 
    *CX = 0007h is a full cursor*
 
    *CX = 2607h is an invisible cursor*
    
    Main Interrupt:

    |Register|Data|
    |--------|----|
    |AH|01h|
    |CH|Initial Scan Line|
    |CL|Final Scan Line|


  - ### AH 02h (Set Cursor Position)
 
    Main Interrupt:

    |Register|Data|
    |--------|----|
    |AH|02h|
    |BH|Page (``0-3 in modes 2 & 3``  ``0 - 7 in modes 0 & 1``    ``0 on graphics modes``)|
    |DH|Row (X)|
    |DL|Column (Y)|
    

- ### INT 13h (Disk Access)

  - ### AH 42h (Reading from a Sector with LBA)
    
    Main interrupt:
    
    |Register|Data|
    |--------|----|
    |AH|42h|
    |DL|Drive number|
    |DS:SI|Disk address packet|
    
    Disk address packet:
    
    |Size|Description|
    |----|-----------|
    |Byte|Size of packet (10h or 18h)|
    |Byte|Reserved (0h)|
    |Word|Number of blocks (LBA) to transfer|
    |DWord|Transfer buffer|
    |QWord|Starting block number (LBA)|

    Returns:

    |Register|Description|
    |--------|-----------|
    |CF|Clear if success|
    |AH|00h|
    |CF|Set on error|
    |AH|[Error code](https://www.ctyme.com/intr/rb-0606.htm#Table234)|
  
  - ### AH 43h (Writing to a Sector with LBA)

    Main interrupt:
    
    |Register|Data|
    |--------|----|
    |AH|43h|
    |AL|Write flags|
    |DL|Drive number|
    |DS:SI|Disk address packet|

    Disk address packet:
    
    |Size|Description|
    |----|-----------|
    |Byte|Size of packet (10h or 18h)|
    |Byte|Reserved (0h)|
    |Word|Number of blocks (LBA) to transfer|
    |DWord|Transfer buffer|
    |QWord|Starting block number (LBA)|

    Returns:

    |Register|Description|
    |--------|-----------|
    |CF|Clear if success|
    |AH|00h|
    |CF|Set on error|
    |AH|[Error code](https://www.ctyme.com/intr/rb-0606.htm#Table234)|
