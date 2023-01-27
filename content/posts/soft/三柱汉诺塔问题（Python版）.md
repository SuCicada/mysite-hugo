思路一样

```
def move(n, a, b, c):
    if(n>1):
        move(n-1,a,c,b)
        move(1,a,b,c)
        move(n-1,b,a,c)
    else:
        print a,'-->',c
n = input('input a number') #int(raw_input())
move(n, 'A', 'B', 'C')
```