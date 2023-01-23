 **用的是夹值法** 
```
#include<stdio.h>
int main(){
    int i=1,n=1,m;
    do
    {
        n=i;
        i=i*2;
    printf("%d\n",n);
    } while((i/2)==n);//n是存数，i去趟雷
    putchar('\n');
    m=n;
    for(;;)
    {
        n=m;
        m+=1;
        if((m-1)!=n)
        {
            printf("break");
            break;
        }
        else
            m-=1;
        i=1;
        while(1)
        {
            if(i!=1)
                if((n-i/2)!=m)
                {
                    printf("int  %d\n",m);
                    return;
                }

            printf("!!!\n");
            if(i!=1)
                m=n;//m是存数，n去趟雷
            n=n+i;
            i*=2;
            printf("%d %d\n",m,n);
        }
    }
  getchar();
  return 0;
}

```
**最大值：2147483647**