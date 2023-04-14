1.随机两个不相等的质数p,q
2.计算p和q的乘积n
3.计算n的欧拉函数$\phi(n)$ 
$$
\begin{aligned}
\phi(n)=\phi(pq)&=\phi(p)\phi(q)\\
&\because p为质数,q为质数\\
&\therefore \phi(p)=p-1,\phi(q)=q-1\\
&\therefore\phi(n)=(p-1)\cdot(q-1)
\end{aligned}
$$
4.随机取一个整数e,条件是$1<e<\phi(n)$，且e与$\phi(n)$ 互质,一般取65537
5.计算e对于$\phi(n)$的模反元素$d$,即:
$$
ed\enspace\equiv1\enspace(mod\enspace\phi(n))
$$
可化为：
$$
ex+\phi(n)y\enspace=\enspace1
$$
x即所求的d,用[扩展欧几里得算法](https://zh.wikipedia.org/wiki/%E6%89%A9%E5%B1%95%E6%AC%A7%E5%87%A0%E9%87%8C%E5%BE%97%E7%AE%97%E6%B3%95)来求,python里面可以直接用gmpy2.gcdext()函数
公钥:(n,e) 私钥:(n,d)
明文m: **m=pow(c,d,n)**
密文c: **c=pow(m,e,n)**

**总结：**
1.取两个质数p,q
2.$n=p*q$
3.$\phi n=(p-1)*(q-1)$
4.取质数e,一般为65537
5.算e对于$\phi(n)$的模反元素$d$,python可以用libnum.invmod(e,$\phi(n)$)算出d
6.**m=pow(c,d,n)  c=pow(m,e,n)**

欧拉定理($a$和$n$互质):$a^{\phi(n)}\equiv1(mod\enspace n)$
费马小定理($a$和**质数**$p$互质):$a^{p-1}\equiv1(mod\enspace p)$
