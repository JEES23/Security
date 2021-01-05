# 어셈블리어 분석
## hello.asm
---
```assembly

global    _start                 ;_start가 전역에 있다

section   .text                  ; 여기서부터 .text 섹션          

_start:

    mov       rax, 1             ;system call write
    mov       rdi, 1             ; 표준 출력
    mov       rsi, message       ; message부터 출력 시작
    mov       rdx, 13            ; 13개를 출력
    syscall
    mov       rax, 60            ; system call exit 
    xor       rdi, rdi           ; 사칙연산 -> 0, 나가기
    syscall
    section   .data
message:

    db        "Hello, World", 10    
    ;  Hello, World / 아스키 코드 10 = LF(new line)
```

---
## strlen.asm
---
```assembly
BITS 64

section .text

global _start

strlen:

    mov rax,0                   ; 0을 rax에 입력  
.looplabel:
  
    cmp byte [rdi],0            ; [rid]와 0 비교
    je  .end                    ; 값이 같으면 end로 이동
    inc rdi                     ; rid의 값 1증가
    inc rax                     ; rax의 값 1증가
    jmp .looplabel              ; 반복의 시작 부분으로 이동
.end:

    ret                         ; rax 값을 al에 저장
    
_start:

    mov   rdi, msg              ; msg를 rdi에 입력 
    call  strlen                ; strlen 실행
    add   al, '0'               ; al 값 + 0(-> 아스키코드 48)
    mov  [len],al               ; al 값을 [len]에 입력
    mov   rax, 1                ; system call write
    mov   rdi, 1                ; 표준 출력
    mov   rsi, len              ; len에서부터 출력
    mov   rdx, 2                ; 2만큼 출력
    syscall                    
    mov   rax, 60               ; system call exit
    mov   rdi, 0                ; 나가기
    syscall        

section .data

    msg db "hello",0x ; 16진수 0xA = 아스키코드 10(= LF(new line))
    len db 0,0xA         
```

