> 习题3-7 DNA序列（DNA Consensus String, ACM/ICPC Seoul 2006, UVa1368）
输入m个长度均为n的DNA序列，求一个DNA序列，到所有序列的总Hamming距离尽量
小。两个等长字符串的Hamming距离等于字符不同的位置个数，例如，ACGT和GCGA的
Hamming距离为2（左数第1, 4个字符不同）。
输入整数m和n（4≤m≤50, 4≤n≤1000），以及m个长度为n的DNA序列（只包含字母
A，C，G，T），输出到m个序列的Hamming距离和最小的DNA序列和对应的距离。如有多
解，要求为字典序最小的解。例如，对于下面5个DNA序列，最优解为TAAGATAC。
TATGATAC
TAAGCTAC
AAAGATCC
TGAGATAC
TAAGATGT
（汉明距离：算出 每列与 此列最多量的字符 不同的字符的个数 ，各个列相加）
**Sample Input**
3
5 8
TATGATAC
TAAGCTAC
AAAGATCC
TGAGATAC
TAAGATGT
4 10
ACGTACGTAC
CCGTACGTAG
GCGTACGTAT
TCGTACGTAA
6 10
ATGTTACCAT
AAGTTACGAT
AACAAAGCAA
AAGTTACCTT
AAGTTACCAA
TACTTACCAA
**Sample Output**
TAAGATAC
7
ACGTACGTAA
6
AAGTTACCAA
12

https://uva.onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&page=show_problem&category=830&problem=4114&mosmsg=Submission+received+with+ID+20524214
```
//能解出来就是好的，有时候略微繁琐一些是在所难免的。
//It's not necessary to tack such a toxic attitude around that it's slightly difficult.（不必对其仿佛洪水猛兽。）
#include<iostream>
#include<string.h>
using namespace std;
const int M=50,N=1000;
char dna[M+5][N+5];
int acgt[4][N+5];
int main()
{
    int T;
    cin>>T;
    while(T--)
    {
        int m,n;
        cin>>m>>n;
        int hamming=0;
        memset(acgt,0,sizeof(acgt));
        for(int i=0;i<m;i++)
        {
            for(int j=0;j<n;j++)
            {
                cin>>dna[i][j];
                switch(dna[i][j])
                {
                    case 'A':acgt[0][j]++;break;//注意字典序
                    case 'C':acgt[1][j]++;break;
                    case 'G':acgt[2][j]++;break;
                    case 'T':acgt[3][j]++;break;
                }
            }
        }
        for(int j=0;j<n;j++)
        {
            int maxx=acgt[0][j],nn=0;
            for(int j2=1;j2<4;j2++)
            {
                if(maxx<acgt[j2][j])
                {
                   maxx=acgt[j2][j];
                   nn=j2;
                }
            }
            switch(nn)
            {
                case 0:cout<<'A';break;
                case 1:cout<<'C';break;
                case 2:cout<<'G';break;
                case 3:cout<<'T';break;
            }
            hamming+=(m-acgt[nn][j]);//行数-最大字符数量=单列汉明
        }
        cout<<endl<<hamming<<endl;
    }
    return 0;
}
//AC at 2017/12/22

```
因为物理实验考试，昨日拖码一日。