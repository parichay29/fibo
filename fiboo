section .data
    result_msg db "The nth Fibonacci number is: ", 0

section .bss
    n resd 1        ; Reserve space for the user input (32-bit integer)
    fib resd 1      ; Reserve space for the result (32-bit integer)

section .text
    global _start

_start:
    ; Read the value of n from the user
    mov     eax, 4           ; sys_write syscall number
    mov     ebx, 1           ; file descriptor (stdout)
    lea     ecx, [msg]       ; pointer to the message
    mov     edx, msglen      ; message length
    int     0x80            ; syscall

    ; Read integer input from the user
    mov     eax, 3           ; sys_read syscall number
    mov     ebx, 0           ; file descriptor (stdin)
    lea     ecx, [n]         ; pointer to the input buffer
    mov     edx, 4           ; maximum number of bytes to read
    int     0x80            ; syscall

    ; Convert the ASCII input to an integer
    xor     eax, eax         ; Clear eax (accumulator)
    xor     ebx, ebx         ; Clear ebx (base for multiplication)
    xor     edi, edi         ; Clear edi (will be used as a counter)
.convert_loop:
    mov     bl, byte [n + edi] ; Load the current character
    cmp     bl, 10            ; Check if it's the newline character
    je      .conversion_done  ; If yes, the conversion is complete
    sub     bl, '0'           ; Convert ASCII character to integer (subtract ASCII '0')
    imul    eax, eax, 10      ; Multiply the result by 10
    add     eax, ebx          ; Add the converted digit to the result
    inc     edi               ; Move to the next character
    jmp     .convert_loop     ; Continue the loop
.conversion_done:

    ; Calculate the nth Fibonacci number
    call    fibonacci

    ; Display the result
    mov     eax, 4           ; sys_write syscall number
    mov     ebx, 1           ; file descriptor (stdout)
    lea     ecx, [result_msg]
    mov     edx, result_msg_len
    int     0x80            ; syscall

    ; Exit the program
    mov     eax, 1           ; sys_exit syscall number
    xor     ebx, ebx         ; Exit code 0
    int     0x80            ; syscall

fibonacci:
    ; Recursive function to calculate the nth Fibonacci number
    ; Input: eax - n (positive integer)
    ; Output: eax - nth Fibonacci number

    ; Base case: Fibonacci(0) and Fibonacci(1) are both 1
    cmp     eax, 0
    jle     .fib_base_case
    cmp     eax, 1
    jle     .fib_base_case
    ; Recursive case: Fibonacci(n) = Fibonacci(n-1) + Fibonacci(n-2)
    push    eax             ; Save the value of n
    dec     eax             ; Calculate Fibonacci(n-1)
    call    fibonacci
    mov     ebx, eax        ; Save Fibonacci(n-1) in ebx
    pop     eax             ; Restore the value of n
    sub     eax, 2          ; Calculate Fibonacci(n-2)
    call    fibonacci
    add     eax, ebx        ; Fibonacci(n) = Fibonacci(n-1) + Fibonacci(n-2)
    ret                     ; Return with the result

.fib_base_case:
    mov     eax, 1          ; Return 1 for Fibonacci(0) and Fibonacci(1)
    ret
