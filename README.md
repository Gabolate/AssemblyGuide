# AssemblyGuide
Guide for programming in Assembly. More stuff added as i learn.


# Registers

Assembly has pre-made variables called 'Registers' that can be used for [interrupts](#interrupts-usage), [memory access](#pointers), [store information](#setting-values), etc

Some registers have other versions of themselves, for example, AX is a 16-bit register and AL (8-bit register) is the lower bytes of AX

Here's a list of general purpose registers for 16, 32 and 64 bits:

|8-bits|16-bits|32-bits|64-bits|
|------|-------|-------|-------|
|AL(low)|AX(low)|EAX(low)|RAX|
|AH(high)|AX(low)|EAX(low)|RAX|
|BL(low)|BX(low)|EBX(low)|RBX|
|BH(high)|BX(low)|EBX(low)|RBX|
|CL(low)|CX(low)|ECX(low)|RCX|
|CH(high)|CX(low)|ECX(low)|RCX|
|DL(low)|DX(low)|EDX(low)|RDX|
|DH(high)|DX(low)|EDX(low)|RDX|
|(null)|BP(low)|EBP(low)|RBP|
|(null)|SI(low)|ESI(low)|RSI|
|(null)|DI(low)|EDI(low)|RDI|


There's also another thing to notice, when you are in 16-bits you can only use your registers and the 8-bit ones,

On 32-bit you can access 16-bit registers but not 8-bit, similar in 64-bits you can access 32-bit but not 16-bit or lower

# The Stack

The stack is used for storing temporal information and [calling functions](#calls)

The way on how it works is that it starts at a certain address and it adds or removes bytes as needed (like a pile of books)

There are certain commands to interact with the stack:

- ``push``
  
  Adds data at the top of the stack

  Example:
  
  ```asm
  push AX ; Stores the data inside AX into the stack
  push 30 ; Stores the number 30 into the stack
  push dword 50 ; Stores the number 50 but as a dword (16-bits/2 bytes) into the stack
  ```

- ``pusha``
  
  Adds all the registers' data into the stack
  
  Its like using lots of 'push' commands with the registers (its not always recommended cuz it takes quite a bit of space in the stack)

- ``pop``
  
  Retrieves and removes data from the stack
  
  Example:

  ```asm
  pop AX ; Stores the last 2 bytes added to the stack into AX (16-bits)
  pop dword [0x8000] ; Stores the last 2 bytes added to the stack into the memory address 0x8000
  ```

 - ``popa``
   
   Retrieves all the last registers' data added into the stack

  
  Now, when the 'call' function uses the stack, it pushes the current memory address, and the 'ret' function jumps to the last address stored in the stack
  
  We can do something similar to jump to an specific address, like in this example:

  ```asm
  push 0x8000 ; We push the address we want to jump to
  ret
  ```
  

  There's something **important** to notice when using the stack, when you 'pop' data you have to do it the reversed way of how you 'pushed' them.

  For example, if i pushed AX and then CX, i have to pop CX and then AX, otherwise it won't retrieve the right data
  
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

  Jumps to a label but after the function/label ended execution its capable of returning to where it was originally

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
  ret ; return to where it was before
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

  Used to define 1 or multiple words (2 bytes, equivalent to a 'short'/'ushort' type in C# or a 16-Bit integer)

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

  Used to define 1 or multiple doublewords (4 bytes, equivalent to a 'int'/'uint' type in C# or a 32-Bit integer)

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

  Used to define 1 or multiple quadwords (8 bytes, equivalent to a 'long'/'ulong' type in C# or a 64-Bit integer)

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
  times 510 - ($ - $$) db 0 ; Fills with zeroes since the end of the program until it reaches 510 (used for MBR/bootloaders)
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
  inc byte [aNumber] ; Increments 1 (14 + 1 = 15)

  mov AX, 20
  inc AX ; We should get 21 (20 + 1 = 20)
  ```

- ### Decrement

  Decrements 1 from a register or memory space (Equivalent in C# to variable--;)

  Usage:
  ```asm
  dec variable
  ```

  Example:

  ```asm
  aNumber db 15 ; Declare a byte with a value of 15
  dec byte [aNumber] ; Decrements 1 (15 - 1 = 14) | You need to specify the size of the variable, in this case its a byte

  mov AX, 10
  dec AX ; We get 9 (10 - 1 = 9)
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
  mov AL, byte [firstNumber] ; We can't directly use firstNumber + secondNumber, we have to store at least one of them into a register
  add AL, secondNumber ; Add secondNumber with firstNumber and store it in AL (2 + 1 = 3)
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
  mov AL, byte [firstNumber] ; We can't directly use firstNumber - secondNumber, we have to store at least one of them into a register
  sub AL, secondNumber ; Subtract secondNumber with firstNumber and store it in AL (2 - 1 = 1)
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
  cmp byte [counter], 10 ; Compare the counter with 10 (We have to specify the size here, cuz its not a register)
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

- ### XOR

  Can be used to set a value as 0 without using ``mov`` (Its useful cuz it uses sighly less space)

  Plus, it works for Bitwise operations.

  Usages:
  ```asm
  xor firstValue, secondValue ; used for Bitwise operations
  xor value, value ; better alternative to mov value, 0
  ```

  Example:

  ```asm
  xor ax, ax ; set ax to 0
  ```

# Extra Details

- Real Mode 1MB of RAM

  Sometimes in Real Mode (16-bits) you are limited to 1MB of RAM even when valid 16-bit addresses exceed 1MB

  There is something called 'A20 Line' which is a method that allows the code to bypass the 1MB restriction
  
  I'll probably make someday a tutorial on that either here or somewhere else

- ### Bits
  
  This is used at the start of a program to specify the bits that its going to work with

  (Usually, bootloaders use Real mode and Operating systems use either Protected or Long mode)
  
  ``BITS 16`` = Real mode
  ``BITS 32`` = Protected mode
  ``BITS 64`` = Long mode


- ### MBR small details/notes

  ```asm
  [BITS 16] ; The bootloader uses 16-bit Real Mode
  [org 0x7C00] ; This is to specify that the code starts at memory address 0x7C00
  ```
  must be at the start of the program
  

- ### Pointers

   Brackets ( ``[`` & ``]`` ) are used for pointers (equivalent to ``*`` in C# and ``&`` in C)

   Example:
  
   ```asm
   mov AL, [0x8000] ; This will take the value from 0x8000 and store it in AL, it can also be used with registers but not all of them
   mov AX, [BX] ; Example using a register (BX), it takes the value inside BX as a memory address
   mov [0x8000], AL ; We can also store things into memory
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
    |AL|Video mode flag (``30h if mode is 0-5 and 7``  ``3Fh if mode = 6``  ``20h if mode is > 7``)|

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

  - ### AH 03h (Get Cursor Information)
 
    Main Interrupt:

    |Register|Data|
    |--------|----|
    |AH|03h|
    |BH|Page (``0-3 in modes 2 & 3``  ``0 - 7 in modes 0 & 1``    ``0 on graphics modes``)|

    Returns:

    |Register|Data|
    |--------|----|
    |CH|Initial Scan Line|
    |CL|Final Scan Line|
    |DH|Row (X)|
    |DL|Column (Y)|

  - ### AH 08h (Get Character and Color from Cursor)
 
    Main Interrupt:

    |Register|Data|
    |--------|----|
    |AH|08h|
    |BH|Page (``0-3 in modes 2 & 3``  ``0 - 7 in modes 0 & 1``    ``0 on graphics modes``)|

    Returns:

    |Register|Data|
    |--------|----|
    |AH|Color|
    |AL|Character|


  - ### AH 09h (Write Character on the Cursor Position)
 
    Main Interrupt:

    |Register|Data|
    |--------|----|
    |AH|09h|
    |AL|Character|
    |BH|Page (``0-3 in modes 2 & 3``  ``0 - 7 in modes 0 & 1``    ``0 on graphics modes``)|
    |BL|Color|
    |CX|Number of times to write the character|

  - ### AH 0Ah (Only Write Character on the Cursor Position)
 
    Main Interrupt:

    |Register|Data|
    |--------|----|
    |AH|0Ah|
    |AL|Character|
    |BH|Page (``0-3 in modes 2 & 3``  ``0 - 7 in modes 0 & 1``    ``0 on graphics modes``)|
    |CX|Number of times to write the character|

  - ### AH 0Bh (Change Background Color)
 
    Main Interrupt:

    |Register|Data|
    |--------|----|
    |AH|0Bh|
    |BH|00h|
    |BL|Color|


  - ### AH 0Ch (Set Graphic Pixel)
 
    Main Interrupt:

    |Register|Data|
    |--------|----|
    |AH|0Ch|
    |AL|Color|
    |BH|Page (``0-3 in modes 2 & 3``  ``0 - 7 in modes 0 & 1``    ``0 on graphics modes``)|
    |CX|X|
    |DX|Y|

  - ### AH 0Eh (Teletype Output)

    *I recommend this command for displaying characters*

    Main Interrupt:

    |Register|Data|
    |--------|----|
    |AH|0Eh|
    |AL|Character to write|
    |BH|Page|
    |BL|Foreground color|


  - ### AH 0Dh (Get Graphic Pixel)
 
    Main Interrupt:

    |Register|Data|
    |--------|----|
    |AH|0Dh|
    |BH|Page (``0-3 in modes 2 & 3``  ``0 - 7 in modes 0 & 1``    ``0 on graphics modes``)|
    |CX|X|
    |DX|Y|

    Returns:

    |Register|Data|
    |--------|----|
    |AL|Color|

  - ### AH 13h (Write String)
 
    Main Interrupt:

    |Register|Data|
    |--------|----|
    |AH|13h|
    |AL|Write mode (``Update cursor after writing = 80h``  ``Text uses alternating chars and attributes = 40h``  ``Both modes: C0h``)|
    |BH|Page (``0-3 in modes 2 & 3``  ``0 - 7 in modes 0 & 1``    ``0 on graphics modes``)|
    |BL|Color|
    |CX|String Length|
    |DH|Row (X)|
    |DL|Column (Y)|
    |ES:BP|String position|

- ### INT 12h (Get Memory Size)
  Returns:
  
  |Register|Description|
  |--------|-----------|
  |AX|Number of Kilobytes of memory|


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


- ### INT 16h (Keyboard)

  - ### AH 00h (Get Keystroke)
    
    Main interrupt:
    
    |Register|Data|
    |--------|----|
    |AH|00h|

    Returns:

    |Register|Description|
    |--------|-----------|
    |AH|BIOS Scan code|
    |AL|ASCII Character|

- ### INT 1Ah (System Time)

  - ### AH 00h (Get System Time)
 
    Main interrupt:

    |Register|Data|
    |--------|----|
    |AH|00h|

    Returns:

    |Register|Description|
    |--------|-----------|
    |CX|Higher bytes of the Number of Clock Ticks since Midnight|
    |DX|Lower bytes of the Number of Clock Ticks since Midnight|
    |AL|Midnight flag (If it isn't a value of 0, Midnight passed since the last time the interrupt was used)|

    For reference: Each second is around 18.2 Clock Ticks

  - ### AH 01h (Set System Time)
 
    Main interrupt:

    |Register|Data|
    |--------|----|
    |AH|01h|
    |CX|Higher bytes of the Number of Clock Ticks since Midnight|
    |DX|Lower bytes of the Number of Clock Ticks since Midnight|

    For reference: Each second is around 18.2 Clock Ticks
