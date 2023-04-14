 设:$dp \equiv d\enspace(mod\enspace(p-1))$

$$
\begin{align}
dp \equiv d\enspace(mod\enspace(p-1))\\
d*e \equiv dp*e \enspace(mod\enspace(p-1))\\
d*e \equiv dp*e+k_1(p-1)\tag{1}\\
\\
d*e\equiv 1(mod\enspace \phi(n))\\
d*e\equiv 1(mod\enspace (p-1)*(q-1))\\
d*e\equiv k_2(p-1)*(q-1)+1\tag{2}\\
\\
(1)(2)式相等得:\\
dp*e+k_1(p-1)=k_2(p-1)*(q-1)+1\\
dp*e=k_2(p-1)*(q-1)-k_1(p-1)+1\\
dp*e=(p-1)*[k_2(p-1)-k_1]+1\\
\because[k_2(p-1)-k_1]为一个整数\\
\therefore 用k来代替[k_2(p-1)-k_1]\\
\therefore dp*e=k*(p-1)+1\tag{3}\\
\\
\because dp \equiv d\enspace(mod\enspace(p-1))\\
\therefore dp<(p-1)\\
\therefore dp*e=k*(p-1)+1<(p-1)*e\\
\therefore k+\frac{1}{p-1}<e\\
\therefore k\in[1,e)\\
\\
\star\quad\therefore dp*e=k*(p-1)+1\quad(k\in[1,e))\tag{4}\\
\end{align}
$$

继续进行推导
$$
\begin{align}
设任意数a\\
设:(a^{dp*e})\%p=?\\
\because dp*e=k*(p-1)+1\\
\therefore (a^{k*(p-1)+1})\%p=?\\
(a^{k*(p-1)}\cdot a)\%p=?\\
[(a^{k*(p-1)})\%p]\cdot(a\%p)=?\\
((a^k)^{p-1})\%p\cdot(a\%p)=?\\
\because 费马小定理\enspace a^{p-1}\equiv 1(mod\enspace p)(p为质数)\\
\therefore ((a^k)^{p-1})\%p\cdot(a\%p)=1\cdot (a\%p)=a\%p\\
\therefore ?=a\%p\\
\therefore (a^{dp*e})\%p=a\%p\tag{1}\\
\\
设:(a^{dp*e})\%n=x\\
(a^{dp*e})+kn=x\\
\because k为任意整数\\
\therefore 可化为(a^{dp*e})-kn=x\\
(a^{dp*e})-k(p*q)=x\\
(a^{dp*e})\%p-k(p*q)\%p=x\%p\\
(a^{dp*e})\%p-0=x\%p\\
(a^{dp*e})\%p=x\%p\\
\therefore (a^{dp*e})\equiv x (mod\enspace p)\\
\therefore (a^{dp*e})\%p=x\\
\because (a^{dp*e})\%n=x\\
\therefore (a^{dp*e})\%p=(a^{dp*e})\%n\\
\because (1)\\
\therefore (a^{dp*e})\%n=a\%p\\
\therefore (a^{dp*e})\%n=a+kp\\
\\
\star \therefore (a^{dp*e})\%n-a=kp\tag{2}\\
\end{align}
$$
(2)式即为最终的结论
$\star \therefore p=gcd(((a^{dp*e})\%n-a),n)$
可以转换成如下python语言:
1.p=libnum.gcd(pow(a,dp\*e,n)-a,n)
2.p=gmpy2.gcd(pow(a,dp\*e,n)-a,n)
其中,a可以是任意整数








