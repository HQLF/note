# ezRSA

下载附件，文件中告诉了我们已知条件：

p和q为两个随机的素数，e=65537,n=p*q,c=pow(m,e,n),m为flag从字符转换为数字而来,我们的目的是求m，即密文

启动实例：

![img](https://raw.githubusercontent.com/HQLF/Picture/main/ezRSA.png)

则知道了p、q、c的值

根据RSA非对称加密算法:

φ(n) = (p-1)(q-1)

e*d&φ(n)=1

m=pow(c,d,n)

因为已经知道了p、q、c、e的值，则可以计算出剩下的值，通过python脚本来实现

```py
import libnum
from Crypto.Util.number import *
#两个要用到的库

e = 
p = 
q = 
c =
#已知条件




n = p*q
n1 = (p-1)*(q-1)#即φ(n)
d = libnum.invmod(e,n1)#这个函数用来求e对于φ(n)的模反元素d
m = pow(c, d, n)
flag = long_to_bytes(m)#把纯数字的密文m转换为字符，即flag
print(flag)
```

最后得出flag即可





