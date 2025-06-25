# Basic Boot:
```Assembly
org 700h  
  
mov AH, 0Eh  
mov AL, 'H'  
int 10h  
  
jajco db 'H'  
  
times 510-($-$$) db 0  
dw 0xAA55
```
# Printing a Boot Message:
```Assembly
org 0x7c00  
  
mov BX, message  
  
print_loop:  
    mov AL, [BX]  
    cmp AL, 0  
    je $ ; infinite loop (stops)  
    mov AH, 0Eh      ; teletype output (int10h)  
    int 10h  
    inc BX  
    jmp print_loop  
  
message db 'Booting HylaOS', 0  
  
times 510-($-$$) db 0  
dw 0xAA55
```

# Reading from Drive + Error Handling
```Assembly
org 0x7c00  
  
mov AH, 02h  
mov AL, 02h  
mov DL, 00h  
mov CH, 00h  
mov CL, 02h  
mov DH, 00h  
mov BX, 0x0500  
mov AX, 0  
mov ES, AX  
int 13h  
  
jc disk_error  
mov BX, msg_success  
jmp print  
  
disk_error:  
    mov BX, msg_error  
    jmp print  
  
print:  
    mov AL, [BX]  
    cmp AL, 0  
    je $  
    mov AH, 0Eh  
    int 10h  
    inc BX  
    jmp print  
  
msg_success db 'Disk read successful.', 0  
msg_error db 'Disk read error!', 0  
  
times 510-($-$$) db 0  
dw 0xAA55
```

Works on mac only if compiled and ran with: 
```
dd if=/dev/zero bs=512 count=2 of=disk.img
dd if=boot.bin of=disk.img conv=notrunc
```

```
qemu-system-x86_64 -drive format=raw,file=disk.img -m 32M
```


**GLOBAL DESCRIPTOR TABLE**
![[Pasted image 20250612110837.png]]
```Assembly
org 0x7C00;  
  
[bits 16]  
  
mov BX, 0x7E00  
mov AH, 02h  
mov AL, 3  
mov CH, 0  
mov CL, 0x02  
mov DH, 0  
mov DL, 0x80  
int 13h  
  
cli  
lgdt [gdt_descriptor]  
mov eax, cr0  
or eax, 1  
mov cr0, eax  
  
jmp CODE_SEG:start_protected      
  
[bits 32]  
start_protected:  
    mov AX, DATA_SEG  
    mov DS, AX  
    mov ES, AX  
    mov GS, AX  
    mov FS, AX  
    mov SS, AX  
  
    jmp CODE_SEG:0x7E00  
  
;GDT  
gdt:start:  
gdt_null:  
dd 0x0  
dd 0x0  
  
gdt_code:  
dw 0xffff ; Limit  
dw 0x0 ; Baza 0-15  
db 0x0 ; Baza 16-23  
db 10010010b ; flagi typu  
db 11001111b ; flagi typu+reszta limitu  
db 0x0  
  
gdt_data:  
dw 0xffff ; Limit  
dw 0x0 ; Baza 0-15  
db 0x0 ; Baza 16-23  
db 10010010b ; flagi typu  
db 11001111b ; flagi typu+reszta limitu  
db 0x0  
  
gdt_end:  
  
gdt_descriptior:  
dw gdt_end - gdt_start - 1  
dd gdt_start  
  
CODE_SEG equ gdt_code - gdt_start  
DATA_SEG equ gdt_data - gdt_start  
  
times 510-($-$$) db 0  
dw 0xAA55
```

Kernel:
```C
void kernel_main(void) {  
    volatile char *video = (volatile char*)0xB8000;  
    video[0] = 'A';  
    video[1] = 0xf;  
    video[3998] = 'Z';  
    video[3999] = 0xD;  
    while(1);  
}
```

`nasm -f bin GDT.asm -o main.bin`

`gcc -ffreestanding -m32 -c kernel.c -o kernel.o` <- to compile kernel

`ld -Ttext 0x7E00 -o kernel.bin --oformat binary kernel.o` <- to make it bin
