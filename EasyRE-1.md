### EasyRE-1

1.下载附件

2.把附件用IDA打开:

![img](https://raw.githubusercontent.com/HQLF/Picture/main/EasyRE-1-1.png)

打开上方的ELF64程序，下方的进程类型默认即可

打开后窗口如下：

![img](https://raw.githubusercontent.com/HQLF/Picture/main/EasyRE-1-2.png)

打开字符串窗口：

![img](https://raw.githubusercontent.com/HQLF/Picture/main/EasyRE-1-3.png)

得到如下视图：

![img](https://raw.githubusercontent.com/HQLF/Picture/main/EasyRE-1-4.png)

看到Flag = flag{The_Flag_IS_True} 字符，去看它在哪里被用到:

![img](https://raw.githubusercontent.com/HQLF/Picture/main/EasyRE-1-5.png)

![img](https://raw.githubusercontent.com/HQLF/Picture/main/EasyRE-1-10.png)

找到了它在passwd函数中被使用到了

跳转到passwd函数:

![img](https://raw.githubusercontent.com/HQLF/Picture/main/EasyRE-1-6.png)

因为看不懂汇编语言，所以F5反汇编查看伪代码：

![img](https://raw.githubusercontent.com/HQLF/Picture/main/EasyRE-1-7.png)

理解这段代码的含义

可以知道了passwd函数用到了四个值：_int64类型的a1,char类型的a2,a3,a4

i从0开始到21

如果i为偶数，那么a1的第i个值会加上a2

如果i为奇数且不被3整除，那么a1的第i个值会加上a4

如果i为奇数且被3整除，那么a1的第i个值会加上a3

进行了这样的相加后，每个值会与key的每个值比较，如果相同则继续循环，如果不相同则直接跳出循环并输出flag is false,如果全部相同,则输出flag is true

可以知道，如果我们输入了正确的flag,这个flag的每个值在进行了如上的加密规则后，与key的每个值比较，如果全部与key的值相同，那么我们的flag就是正确的

为了得到正确的flag,我们去看key的值:

![img](https://raw.githubusercontent.com/HQLF/Picture/main/EasyRE-1-8.png)

为了进行计算，我们把key中的每个值转为10进制：

![img](https://raw.githubusercontent.com/HQLF/Picture/main/EasyRE-1-9.png)

因为在之前16进制时，我们看到了几个元素的值为0FFFFFF开头的，这个正确的值应该为负数，而IDA这里默认这个数为一个正数，而把这个负数的符号位也算入了数中，所以得到了一个特别大的数，所以我们应该把这些值变成负的10进制数，而不是一个特别大的数,因此要把这个数的2进制形式表示出来，这个即为该数的原码，原码的第一位为符号位，1即为负，0为正，我们把除了符号位的其他数都取反，得到这个数的反码，最后再把反码+1，得到这个数的补码,取这个补码的除了符号位以外的2进制数，即为这个数的绝对值，在前面加个负号即为正确的key的值,那么我们可以写程序来快速计算

```c
#include <stdio.h>
#include <math.h>
#define ll long long
void solve()
{
    ll y1[100]={0};
    ll y2[100]={0};
    ll a;
    scanf("%lld",&a);

    for(int i=0;i<=63;i++)
    {
        y1[i]=a%2;
        a/=2;
    }
    for(int i=0;i<=63;i++)
    {
        y2[i]=y1[63-i];
    }
    ll y3[100]={0};
    int fl=0;
    int l=0;
    for(int i=0;i<=63;i++)
    {
        if(y2[i]==1)
        {
            fl=1;
        }
        if(fl==1)
        {
            y3[l]=y2[i];
            l++;
        }
    }
    l--;
    ll f[100]={0};
    for(int i=0;i<=l;i++)
    {
        if(y3[i]==1)
        {
            f[i]=0;
        }
        else if(y3[i]==0)
        {
            f[i]=1;
        }
    }
    fl=0;
    ll f1[100]={0};
    int l1=0;
    for(int i=0;i<=l;i++)
    {
        if(f[i]==1)
        {
           fl=1;
        }
        if(fl==1)
        {
            f1[l1]=f[i];
            l1++;
        }
    }
    l1--;
    ll sum=0;
    for(int i=l1;i>=0;i--)
    {
        if(f1[i]==1)
        {
            sum+=((ll)pow(2,l1-i));
        }
    }
    sum++;
    printf("-%lld\n",sum);
}
int main()
{
    int t;
    scanf("%d",&t);
    while (t--)
    {
        solve();
    }
    return 0;
}
```

以上是我写的C语言程序，先输入运行的次数，然后输入IDA中转换错误的10进制数，就会输出转换正确的10进制负数

最后得到正确的key数组的每个元素

那么我们现在只要知道a1的每个元素加了什么，并用key的那个对应的元素去减去加的值，就可以得到原来的值了,可以写个简单的程序来判断加了什么

因为加的是a2,a3,a4，所以我们看附件中的main函数并F5反汇编:

![img](https://raw.githubusercontent.com/HQLF/Picture/main/EasyRE-1-11.png)

可以看到a2=11,a3=45,a4=14

那么我们可以写程序来让key的每个值去减去这些值

```c
#include <stdio.h>
#define ll long long


void solve()
{
    ll key[100]={ };//已知条件
    for(int i=0;i<=21;i++)
    {
        if(i&1)
        {
            if (i%3)
            {
                key[i]-=14;
            }
            else
            {
                key[i]-=45;
            }
        }
        else
        {
            key[i]-=11;
        }
    }
    for(int i=0;i<=50;i++)
    {
        printf("%lld ",key[i]);
    }
}

int main()
{
    solve();
    return 0;
}





```

以上是我写的程序，这个程序会直接输出key的每个值减去相应的加密值后的值，这里用key1数组表示

最后，因为main函数中，看到a1的第4个值为103，而我们得到的key1的第四个值为-153，那我们就要搞懂这个负数是这么变成103的，通过把这个数输入到计算机中，我们得到这个数的二进制为：1111111111111111111111111111111111111111111111111111111101100111

把前面的1去掉：01100111

这个值即为103，所以之后所有的负数都通过这个规则即可求得正确的值，我们可以写程序来完成:

```c
#include <stdio.h>
#include <math.h>
#include <stdlib.h>
#define ll long long

void solve()
{
    ll y1[100]={0};
    ll a;
    scanf("%lld",&a);
    a=llabs(a);
    for(int i=0;i<=63;i++)
    {
        y1[i]=a%2;
        a/=2;
    }
    for(int i=0;i<=63;i++)
    {
        if(y1[i]==1) y1[i]=0;
        else if(y1[i]==0) y1[i]=1;
    }
    ll f[100]={0};
    for(int i=0;i<=63;i++)
    {
        f[i]=y1[63-i];
    }

    int fl=0;
    int l1=0;
    ll f1[100]={0};
    for(int i=0;i<=63;i++)
    {
        if(f[i]==0) fl=1;
        if(fl==1)
        {
            f1[l1]=f[i];
            l1++;
        }
    }
    l1--;
    ll sum=0;
    for(int i=0;i<=l1;i++)
    {
        if(f1[i]==1)
        {
            sum+=((ll)pow(2,l1-i));
        }
    }
    sum++;
    printf("%lld\n",sum);
}
int main()
{
    int t;
    scanf("%d",&t);
    while (t--)
    {
        solve();
    }
    return 0;
}





```

以上是我写的程序，输入运算的次数，然后输入要转换的负数，就会输出对应转换规则的正数

至此，我们得到了a1的所有值，把这些十进制数转换为字符，即可得到最后的flag















