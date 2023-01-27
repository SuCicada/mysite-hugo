如果ax中的数是奇数：bx为0
否则bx为1

```
; judge which the number in ax is odd or even  
assume cs:code
code segment
    
    mov ax,[bx]
    mov cx,ax
    mov bx,0
s:
    loop i      ; if can't loop , cx is 1  
                ; every loop, cx -= 2
    ; odd
    mov bx,0
    loop k
i:    
    ; sub cx,1
    loop s    
    ;  even
    mov bx,1
    ; loop k
k:
    ;over
    mov ax,4c00h
    int 21h

code ends
end
```

