https://uva.onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&page=show_problem&category=829&problem=1023&mosmsg=Submission+received+with+ID+20388214
> A common typing error is to place the
hands on the keyboard one row to the
right of the correct position. So ‘Q’ is
typed as ‘W’ and ‘J’ is typed as ‘K’ and
so on. You are to decode a message typed in this manner.
Input
Input consists of several lines of text. Each line may contain digits, spaces, upper case letters (except
Q, A, Z), or punctuation shown above [except back-quote (‘)]. Keys labelled with words [Tab, BackSp,
Control, etc.] are not represented in the input.
Output
You are to replace each letter or punction symbol by the one immediately to its left on the ‘QWERTY’
keyboard shown above. Spaces in the input should be echoed in the output.
Sample Input
O S, GOMR YPFSU/
Sample Output
I AM FINE TODAY.

```
#include<stdio.h>
#include<iostream>
#include<string.h>
using namespace std;
int main()
{
    char *s="`1234567890-=QWERTYUIOP[]\\ASDFGHJKL;'ZXCVBNM,./";
    char c;
    while((int)(c=getchar())!=EOF)
    {
        char *p;
        if(p=strchr(s,c))
            cout<<*(p-1);
        else
            cout<<c;
    }
    return 0;
}

```
书上的例题看的书写了一个晚上，堪忧