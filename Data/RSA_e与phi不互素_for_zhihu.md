## 前言

我们知道，在 RSA 系统中，解密依赖于欧拉定理


<img src="https://www.zhihu.com/equation?tex=a^{\phi(p)}\equiv1\;mod\;p\;,\;(a,p)=1\\
" alt="a^{\phi(p)}\equiv1\;mod\;p\;,\;(a,p)=1\\
" class="ee_img tr_noresize" eeimg="1">

以及同余等式


<img src="https://www.zhihu.com/equation?tex=ed\equiv1\;mod\;\phi(n)\\
" alt="ed\equiv1\;mod\;\phi(n)\\
" class="ee_img tr_noresize" eeimg="1">

,那么在已知


<img src="https://www.zhihu.com/equation?tex=c\equiv\;m^{e}\;mod\;n
" alt="c\equiv\;m^{e}\;mod\;n
" class="ee_img tr_noresize" eeimg="1">

下，容易得到


<img src="https://www.zhihu.com/equation?tex=c^{d}\equiv\;(m^{e})^{d}\;mod\;n\\
" alt="c^{d}\equiv\;(m^{e})^{d}\;mod\;n\\
" class="ee_img tr_noresize" eeimg="1">


<img src="https://www.zhihu.com/equation?tex=ed=1+k\phi(n)\\
" alt="ed=1+k\phi(n)\\
" class="ee_img tr_noresize" eeimg="1">


<img src="https://www.zhihu.com/equation?tex=c^{d}\equiv\;m*m^{k\phi(n)}\;\equiv\;m*1\;mod\;n\\
" alt="c^{d}\equiv\;m*m^{k\phi(n)}\;\equiv\;m*1\;mod\;n\\
" class="ee_img tr_noresize" eeimg="1">

但是当 <img src="https://www.zhihu.com/equation?tex=e,phi" alt="e,phi" class="ee_img tr_noresize" eeimg="1"> 不互素的情况下，私钥 <img src="https://www.zhihu.com/equation?tex=d" alt="d" class="ee_img tr_noresize" eeimg="1"> 就无法正常计算，在此之下想要顺利求解明文 <img src="https://www.zhihu.com/equation?tex=m" alt="m" class="ee_img tr_noresize" eeimg="1"> ,衍生了一系列问题，本文基于笔者日常中遇到的相关题型，给出对应的处理方法

## 从简单情况出发

我们不妨令


<img src="https://www.zhihu.com/equation?tex=gcd(e,\phi)=t\\
" alt="gcd(e,\phi)=t\\
" class="ee_img tr_noresize" eeimg="1">


<img src="https://www.zhihu.com/equation?tex=te^{'}=e\\
" alt="te^{'}=e\\
" class="ee_img tr_noresize" eeimg="1">

那么考虑转化为公钥指数为 <img src="https://www.zhihu.com/equation?tex=e^{'}" alt="e^{'}" class="ee_img tr_noresize" eeimg="1"> 的 RSA，有


<img src="https://www.zhihu.com/equation?tex=e^{'}d^{'}\equiv1mod\phi(n)\\
" alt="e^{'}d^{'}\equiv1mod\phi(n)\\
" class="ee_img tr_noresize" eeimg="1">


<img src="https://www.zhihu.com/equation?tex=c\equiv\;m^{e}\equiv\;m^{te^{'}}\;mod\;n\\
" alt="c\equiv\;m^{e}\equiv\;m^{te^{'}}\;mod\;n\\
" class="ee_img tr_noresize" eeimg="1">


<img src="https://www.zhihu.com/equation?tex=c^{d^{'}}\equiv\;m^{te^{'}d^{'}}\;\equiv\;m^{t}\;mod\;n\\
" alt="c^{d^{'}}\equiv\;m^{te^{'}d^{'}}\;\equiv\;m^{t}\;mod\;n\\
" class="ee_img tr_noresize" eeimg="1">

显然此时我们能够计算


<img src="https://www.zhihu.com/equation?tex=c^{d^{'}}\;mod\;n\\
" alt="c^{d^{'}}\;mod\;n\\
" class="ee_img tr_noresize" eeimg="1">

如果 <img src="https://www.zhihu.com/equation?tex=t" alt="t" class="ee_img tr_noresize" eeimg="1"> 不是很大（个位数），当 <img src="https://www.zhihu.com/equation?tex=m^{t}<n" alt="m^{t}<n" class="ee_img tr_noresize" eeimg="1"> 的时候，我们直接能得到


<img src="https://www.zhihu.com/equation?tex=c^{d^{'}}\;mod\;n=m^{t}\\
" alt="c^{d^{'}}\;mod\;n=m^{t}\\
" class="ee_img tr_noresize" eeimg="1">

，直接开 <img src="https://www.zhihu.com/equation?tex=t" alt="t" class="ee_img tr_noresize" eeimg="1"> 次方就能得到 <img src="https://www.zhihu.com/equation?tex=m" alt="m" class="ee_img tr_noresize" eeimg="1"> 

如果 <img src="https://www.zhihu.com/equation?tex=m^{t}" alt="m^{t}" class="ee_img tr_noresize" eeimg="1"> 不那么大的话，我们姑且还能尝试爆破


<img src="https://www.zhihu.com/equation?tex=c^{d^{'}}=kn+m^{t}\\
" alt="c^{d^{'}}=kn+m^{t}\\
" class="ee_img tr_noresize" eeimg="1">

，但是如果数据不支持，大到爆破不了，我们就得进一步观察已有数据中的代数结构了

## 如果 <img src="https://www.zhihu.com/equation?tex=e" alt="e" class="ee_img tr_noresize" eeimg="1"> 同时与 <img src="https://www.zhihu.com/equation?tex=p-1" alt="p-1" class="ee_img tr_noresize" eeimg="1"> ， <img src="https://www.zhihu.com/equation?tex=q-1" alt="q-1" class="ee_img tr_noresize" eeimg="1"> 互质（CRT 加速 RSA 问题）

这里也就是在 RSA 中使用 CRT 算法加速运算的例子，我们都来讲讲如何操作

> 为什么要用 CRT？

在 RSA 的计算中，我们有


<img src="https://www.zhihu.com/equation?tex=ed\equiv1\;mod\;\phi(n)\\
" alt="ed\equiv1\;mod\;\phi(n)\\
" class="ee_img tr_noresize" eeimg="1">


<img src="https://www.zhihu.com/equation?tex=c\equiv\;m^d\;modn\\
" alt="c\equiv\;m^d\;modn\\
" class="ee_img tr_noresize" eeimg="1">

，我们知道 <img src="https://www.zhihu.com/equation?tex=n=pq" alt="n=pq" class="ee_img tr_noresize" eeimg="1"> ， <img src="https://www.zhihu.com/equation?tex=n" alt="n" class="ee_img tr_noresize" eeimg="1"> 往往非常大，那么计算上面两个式子就非常的消耗时间，而且也需要 <img src="https://www.zhihu.com/equation?tex=(e,\phi)=1" alt="(e,\phi)=1" class="ee_img tr_noresize" eeimg="1"> ,我们注意到 <img src="https://www.zhihu.com/equation?tex=p,q" alt="p,q" class="ee_img tr_noresize" eeimg="1"> 也是两个大质数，很有可能 <img src="https://www.zhihu.com/equation?tex=e" alt="e" class="ee_img tr_noresize" eeimg="1"> 与 <img src="https://www.zhihu.com/equation?tex=p-1\;\;(即\phi(p))" alt="p-1\;\;(即\phi(p))" class="ee_img tr_noresize" eeimg="1">  ;  <img src="https://www.zhihu.com/equation?tex=q-1\;\;(即\phi(q))" alt="q-1\;\;(即\phi(q))" class="ee_img tr_noresize" eeimg="1"> 也互素，那么我能就能把问题转化到模 <img src="https://www.zhihu.com/equation?tex=p,q" alt="p,q" class="ee_img tr_noresize" eeimg="1">  下来讨论，下面是推导过程


<img src="https://www.zhihu.com/equation?tex=由c\equiv\;m^{e}\;mod\;n\\
" alt="由c\equiv\;m^{e}\;mod\;n\\
" class="ee_img tr_noresize" eeimg="1">


<img src="https://www.zhihu.com/equation?tex=c\equiv\;m^e\;mod\;p\\
" alt="c\equiv\;m^e\;mod\;p\\
" class="ee_img tr_noresize" eeimg="1">


<img src="https://www.zhihu.com/equation?tex=c\equiv\;m^e\;mod\;q\\
" alt="c\equiv\;m^e\;mod\;q\\
" class="ee_img tr_noresize" eeimg="1">

这是由于 <img src="https://www.zhihu.com/equation?tex=q|n\;;p|n" alt="q|n\;;p|n" class="ee_img tr_noresize" eeimg="1"> 也就是模的传递性，那么我们转过来计算在模 <img src="https://www.zhihu.com/equation?tex=p,q" alt="p,q" class="ee_img tr_noresize" eeimg="1"> 下的私钥 <img src="https://www.zhihu.com/equation?tex=d_p;d_q" alt="d_p;d_q" class="ee_img tr_noresize" eeimg="1"> ，即


<img src="https://www.zhihu.com/equation?tex=edp\equiv1\;mod\;\phi(p)\\
" alt="edp\equiv1\;mod\;\phi(p)\\
" class="ee_img tr_noresize" eeimg="1">


<img src="https://www.zhihu.com/equation?tex=ed_q\equiv1\;mod\;\phi(q)\\
" alt="ed_q\equiv1\;mod\;\phi(q)\\
" class="ee_img tr_noresize" eeimg="1">

那么在解密的时候就有


<img src="https://www.zhihu.com/equation?tex=c^{d_p}\equiv\;m^{ed_p}\equiv\;m^{1+k\phi(p)}\;\equiv\;m\;mod\;p\\
" alt="c^{d_p}\equiv\;m^{ed_p}\equiv\;m^{1+k\phi(p)}\;\equiv\;m\;mod\;p\\
" class="ee_img tr_noresize" eeimg="1">


<img src="https://www.zhihu.com/equation?tex=c^{d_q}\equiv\;m^{ed_q}\equiv\;m^{1+k\phi(q)}\;\equiv\;m\;mod\;q\\
" alt="c^{d_q}\equiv\;m^{ed_q}\equiv\;m^{1+k\phi(q)}\;\equiv\;m\;mod\;q\\
" class="ee_img tr_noresize" eeimg="1">


<img src="https://www.zhihu.com/equation?tex=记c^{d_p}\equiv\;m\;mod\;p\;\;=m_1\;\;c^{d_q}\equiv\;m\;mod\;q\;\;=m_2\\
" alt="记c^{d_p}\equiv\;m\;mod\;p\;\;=m_1\;\;c^{d_q}\equiv\;m\;mod\;q\;\;=m_2\\
" class="ee_img tr_noresize" eeimg="1">

这里是因为 <img src="https://www.zhihu.com/equation?tex=m" alt="m" class="ee_img tr_noresize" eeimg="1"> 可能比 <img src="https://www.zhihu.com/equation?tex=n" alt="n" class="ee_img tr_noresize" eeimg="1"> 要大，所以我们无法直接解出来，然后我们根据中国剩余定理可以构造出一个对 <img src="https://www.zhihu.com/equation?tex=m" alt="m" class="ee_img tr_noresize" eeimg="1"> 的解，如下

由第二个同余式不妨设


<img src="https://www.zhihu.com/equation?tex=m=m_2+hq\;\\
" alt="m=m_2+hq\;\\
" class="ee_img tr_noresize" eeimg="1">

我们进而只需要解出来 <img src="https://www.zhihu.com/equation?tex=h" alt="h" class="ee_img tr_noresize" eeimg="1"> ,把这个式子带入另一个同余式


<img src="https://www.zhihu.com/equation?tex=m_2+hq\equiv\;m_1mod\;p\\
" alt="m_2+hq\equiv\;m_1mod\;p\\
" class="ee_img tr_noresize" eeimg="1">


<img src="https://www.zhihu.com/equation?tex=hq\equiv(m_1-m_2)\;mod\;p\\
" alt="hq\equiv(m_1-m_2)\;mod\;p\\
" class="ee_img tr_noresize" eeimg="1">

我们只需计算


<img src="https://www.zhihu.com/equation?tex=I_q\equiv\;q^{-1}\;mod\;p\\
" alt="I_q\equiv\;q^{-1}\;mod\;p\\
" class="ee_img tr_noresize" eeimg="1">

那么有


<img src="https://www.zhihu.com/equation?tex=h\equiv\;I_q(m_1-m_2)\;mod\;p\\
" alt="h\equiv\;I_q(m_1-m_2)\;mod\;p\\
" class="ee_img tr_noresize" eeimg="1">

得到 <img src="https://www.zhihu.com/equation?tex=h" alt="h" class="ee_img tr_noresize" eeimg="1"> 之后回代即可成功求出 <img src="https://www.zhihu.com/equation?tex=m" alt="m" class="ee_img tr_noresize" eeimg="1"> 

可见这里我们的计算都是在模 <img src="https://www.zhihu.com/equation?tex=p,q" alt="p,q" class="ee_img tr_noresize" eeimg="1"> 的式子中进行的，乘法运算的次数少了不少，实现了加速

说回正题，从上面不难看出，我们自始至终都没有尝试计算 <img src="https://www.zhihu.com/equation?tex=ed\equiv1\;mod\;\phi(n)" alt="ed\equiv1\;mod\;\phi(n)" class="ee_img tr_noresize" eeimg="1"> ，成功回避了这个问题

来个例子--**黑盾杯 2020-Factor**

```python
n = 3454083680130687060405946528826790951695785465926614724373
e = 3
c = 1347530713288996422676156069761604101177635382955634367208
gcd(m, n) = 1
```

这里 n 不是很大，简单分解后能得到三个素因子，我们记为 pqr，如下

```python
p=11761833764528579549
q=17100682436035561357
r=17172929050033177661
print(n-p*q*r) # 0
print(gcd(e,phi)) # 3
print(gcd(e,p-1)) # 1
print(gcd(e,q-1)) # 3
print(gcd(e,r-1)) # 1
print(gcd(e,(p-1)*(r-1))) # 1

```

简单检查了关系，如上，这题方法很多，在上面的解法下即利用 <img src="https://www.zhihu.com/equation?tex=(e,p-1)=(e,r-1)=1" alt="(e,p-1)=(e,r-1)=1" class="ee_img tr_noresize" eeimg="1"> 来使用 CRT 求解，代码如下

```py
#sage
dp=inverse(e,p-1)
dr=inverse(e,r-1)
m1=pow(c,dp,p)
m2=pow(c,dr,r)
Ir=pow(r,-1,p)
h=pow(Ir*(m1-m2),1,p)
m=m2+h*r
print(long_to_bytes(m))
# CMISCCTF{3_RSA}
```

这里也能转化到在模 <img src="https://www.zhihu.com/equation?tex=pr" alt="pr" class="ee_img tr_noresize" eeimg="1"> 下尝试求解，但是那对明文的大小有一定要求，我们还是使用通法，这里只要能找到两个满足要求的因子就行，但是这不是什么时候都能生效，请看下题

**强网杯 2022--[ASR]**

```python
from Crypto.Util.number import getPrime
from secret import falg
pad = lambda s:s + bytes([(len(s)-1)%16+1]*((len(s)-1)%16+1))

n = getPrime(128)**2 * getPrime(128)**2 * getPrime(128)**2 * getPrime(128)**2
e = 3
flag = pad(flag)
print(flag)
assert(len(flag) >= 48)
m = int.from_bytes(flag,'big')
c = pow(m,e,n)
print(f'n = {n}')
print(f'e = {e}')
print(f'c = {c}')
n = 8250871280281573979365095715711359115372504458973444367083195431861307534563246537364248104106494598081988216584432003199198805753721448450911308558041115465900179230798939615583517756265557814710419157462721793864532239042758808298575522666358352726060578194045804198551989679722201244547561044646931280001
e = 3
c = 945272793717722090962030960824180726576357481511799904903841312265308706852971155205003971821843069272938250385935597609059700446530436381124650731751982419593070224310399320617914955227288662661442416421725698368791013785074809691867988444306279231013360024747585261790352627234450209996422862329513284149

```

注意到这里特别对 m 进行了填充，就是保证 m 比 n 的因子大，无法转化为在小因子的域下求解，看看下面这段失败的 exp

```python
from Crypto.Util.number import *
e = 3
n = 8250871280281573979365095715711359115372504458973444367083195431861307534563246537364248104106494598081988216584432003199198805753721448450911308558041115465900179230798939615583517756265557814710419157462721793864532239042758808298575522666358352726060578194045804198551989679722201244547561044646931280001
c = 945272793717722090962030960824180726576357481511799904903841312265308706852971155205003971821843069272938250385935597609059700446530436381124650731751982419593070224310399320617914955227288662661442416421725698368791013785074809691867988444306279231013360024747585261790352627234450209996422862329513284149

p = 225933944608558304529179430753170813347
q = 260594583349478633632570848336184053653
r = 218566259296037866647273372633238739089
t = 223213222467584072959434495118689164399

print(gcd(e,p-1)) # 3
print(gcd(e,q-1)) # 1
print(gcd(e,r-1)) # 3
print(gcd(e,t-1)) # 1
dt=inverse(e,t-1)
dq=inverse(e,q-1)
m1=pow(c,dt,t)
m2=pow(c,dq,q)
Iq=pow(q,-1,t)
h=pow(Iq*(m1-m2),1,t)
m=m2+h*q
print(long_to_bytes(m))
```

这里的确找到了满足 <img src="https://www.zhihu.com/equation?tex=(a_i-1,e)=1" alt="(a_i-1,e)=1" class="ee_img tr_noresize" eeimg="1"> 的两个因子，但是我们注意到，若 <img src="https://www.zhihu.com/equation?tex=m=hq+m_2" alt="m=hq+m_2" class="ee_img tr_noresize" eeimg="1"> 又 <img src="https://www.zhihu.com/equation?tex=m_2\in(0,q)" alt="m_2\in(0,q)" class="ee_img tr_noresize" eeimg="1"> ,所以 <img src="https://www.zhihu.com/equation?tex=m\in(hq,(h+1)q)" alt="m\in(hq,(h+1)q)" class="ee_img tr_noresize" eeimg="1"> 我们在上述代码中解出来的 <img src="https://www.zhihu.com/equation?tex=h" alt="h" class="ee_img tr_noresize" eeimg="1"> 满足 <img src="https://www.zhihu.com/equation?tex=h\in(0,t)" alt="h\in(0,t)" class="ee_img tr_noresize" eeimg="1"> ，也就是说，如果 <img src="https://www.zhihu.com/equation?tex=m>(1+t)q" alt="m>(1+t)q" class="ee_img tr_noresize" eeimg="1"> 的时候，这个方法就暂时失效了，这是在多因子 RSA 下特别的问题，注意到我们还有两个因子没有使用，为了也能使用上它们，我们给出更加一般的方法

## 有限域开根

想想我们的核心目标是什么？--求解 <img src="https://www.zhihu.com/equation?tex=m" alt="m" class="ee_img tr_noresize" eeimg="1">  那么我们为什么要找到这个 <img src="https://www.zhihu.com/equation?tex=(a_i-1,e)=1" alt="(a_i-1,e)=1" class="ee_img tr_noresize" eeimg="1"> 的 <img src="https://www.zhihu.com/equation?tex=a_i" alt="a_i" class="ee_img tr_noresize" eeimg="1"> ?重新捋一遍过程，我们就发现，是为了去计算 <img src="https://www.zhihu.com/equation?tex=d_{a_i}" alt="d_{a_i}" class="ee_img tr_noresize" eeimg="1"> 来求解满足 <img src="https://www.zhihu.com/equation?tex=m^e\equiv\;c\;mod\;a_i" alt="m^e\equiv\;c\;mod\;a_i" class="ee_img tr_noresize" eeimg="1"> 的 <img src="https://www.zhihu.com/equation?tex=m\equiv\;c^{d_{a_i}}\;mod\;a_i" alt="m\equiv\;c^{d_{a_i}}\;mod\;a_i" class="ee_img tr_noresize" eeimg="1"> ,换言之，我们还是在求模不同素因子下的 <img src="https://www.zhihu.com/equation?tex=m" alt="m" class="ee_img tr_noresize" eeimg="1"> ,我们不妨想一下， <img src="https://www.zhihu.com/equation?tex=d_{a_i}" alt="d_{a_i}" class="ee_img tr_noresize" eeimg="1"> 真的重要吗？,我们看这个方程


<img src="https://www.zhihu.com/equation?tex=m^e-c\equiv0\;mod\;a_i\\
" alt="m^e-c\equiv0\;mod\;a_i\\
" class="ee_img tr_noresize" eeimg="1">

这不就是在 <img src="https://www.zhihu.com/equation?tex=GF(a_i)" alt="GF(a_i)" class="ee_img tr_noresize" eeimg="1"> 下的方程吗？这里 <img src="https://www.zhihu.com/equation?tex=e" alt="e" class="ee_img tr_noresize" eeimg="1"> 还非常小，完全可以在这个有限域 <img src="https://www.zhihu.com/equation?tex=GF(a_i)" alt="GF(a_i)" class="ee_img tr_noresize" eeimg="1"> 下来开根求解所有符合题意的 <img src="https://www.zhihu.com/equation?tex=m" alt="m" class="ee_img tr_noresize" eeimg="1"> 的取值集合 <img src="https://www.zhihu.com/equation?tex=M_i" alt="M_i" class="ee_img tr_noresize" eeimg="1"> ，显然可能有多解，但是我们可以依次枚举 <img src="https://www.zhihu.com/equation?tex=M_1,M_2,M_3,M_4" alt="M_1,M_2,M_3,M_4" class="ee_img tr_noresize" eeimg="1"> 中的值，由上述分析，若 <img src="https://www.zhihu.com/equation?tex=(a_k-1,e)=1" alt="(a_k-1,e)=1" class="ee_img tr_noresize" eeimg="1"> ，那么 <img src="https://www.zhihu.com/equation?tex=M_k=\left\{m\;mod\;a_k\right\}" alt="M_k=\left\{m\;mod\;a_k\right\}" class="ee_img tr_noresize" eeimg="1"> 否则也有 <img src="https://www.zhihu.com/equation?tex=m\;mod\;a_j\;\in\;M_j" alt="m\;mod\;a_j\;\in\;M_j" class="ee_img tr_noresize" eeimg="1"> 

那么我们枚举这四个集合，一定能找到满足我们需要的


<img src="https://www.zhihu.com/equation?tex=\begin{cases}m_1=m\;mod\;a_1\\m_2=m\;mod\;a_2\\m_3=m\;mod\;a_3\\m_4=m\;mod\;a_4\\\end{cases}
" alt="\begin{cases}m_1=m\;mod\;a_1\\m_2=m\;mod\;a_2\\m_3=m\;mod\;a_3\\m_4=m\;mod\;a_4\\\end{cases}
" class="ee_img tr_noresize" eeimg="1">

这就是中国剩余定理了，我们直接打 CRT 就能解出来 m，而且我们用到了所有的因子，几乎可以保证 <img src="https://www.zhihu.com/equation?tex=m" alt="m" class="ee_img tr_noresize" eeimg="1"> 这里是唯一的，如果不是唯一的，我们确定一下 <img src="https://www.zhihu.com/equation?tex=n=p^{a}q^{b}r^{c}t^{d}" alt="n=p^{a}q^{b}r^{c}t^{d}" class="ee_img tr_noresize" eeimg="1"> 中的 <img src="https://www.zhihu.com/equation?tex=a,b,c,d" alt="a,b,c,d" class="ee_img tr_noresize" eeimg="1"> ，转化到 <img src="https://www.zhihu.com/equation?tex=m" alt="m" class="ee_img tr_noresize" eeimg="1"> 模 <img src="https://www.zhihu.com/equation?tex=p^{a},\;q^{b},\;r^{c},\;t^{d}" alt="p^{a},\;q^{b},\;r^{c},\;t^{d}" class="ee_img tr_noresize" eeimg="1"> 下就行，下面是修改后的 exp

```py
# sage
from Crypto.Util.number import *
from itertools import product

# Given parameters
e = 3
n = 8250871280281573979365095715711359115372504458973444367083195431861307534563246537364248104106494598081988216584432003199198805753721448450911308558041115465900179230798939615583517756265557814710419157462721793864532239042758808298575522666358352726060578194045804198551989679722201244547561044646931280001
c = 945272793717722090962030960824180726576357481511799904903841312265308706852971155205003971821843069272938250385935597609059700446530436381124650731751982419593070224310399320617914955227288662661442416421725698368791013785074809691867988444306279231013360024747585261790352627234450209996422862329513284149

# Prime factors
p = 225933944608558304529179430753170813347
q = 260594583349478633632570848336184053653
r = 218566259296037866647273372633238739089
t = 223213222467584072959434495118689164399

# Create polynomial rings and find roots in each field
R.<x> = Zmod(p)[]
f = x^e - c
res_p = f.monic().roots()

R.<x> = Zmod(q)[]
f = x^e - c
res_q = f.monic().roots()

R.<x> = Zmod(r)[]
f = x^e - c
res_r = f.monic().roots()

R.<x> = Zmod(t)[]
f = x^e - c
res_t = f.monic().roots()

# Iterate through all possible combinations of roots
for (m_p, _) in res_p:
    for (m_q, _) in res_q:
        for (m_r, _) in res_r:
            for (m_t, _) in res_t:
                # Combine using CRT in stages
                # First combine p and q
                m_pq = crt(int(m_p), int(m_q), p, q)
                # Then combine r and t
                m_rt = crt(int(m_r), int(m_t), r, t)
                # Finally combine the two results
                m = crt(m_pq, m_rt, p*q, r*t)

                mes = long_to_bytes(m)
                print(mes)

# b'flag{Fear_can_hold_you_prisoner_Hope_can_set_you_free}\x06\x06\x06\x06\x06\x06'

```

在这个脚本下我们能解决大部分问题，但是当 <img src="https://www.zhihu.com/equation?tex=e" alt="e" class="ee_img tr_noresize" eeimg="1"> 太大的时候我们就得考虑其他的方法来处理了

根据题型，我们有几种应付的思路

**UCSCCTF2025--EzCalculate**

题面

```python
p = 9586253455468582613875015189854230646329578628731744411408644831684238720919107792959420247980417763684885397749546095133107188260274536708721056484419031
q = 8998523072192453101232205847855618180700579235012899613083663121402246420191771909612939404791268078655630846054784775118256720627970477420936836352759291
n = p * q
e = 65536
c = 74962027356320017542746842438347279031419999636985213695851878703229715143667648659071242394028952959096683055640906478244974899784491598741415530787571499313545501736858104610426804890565497123850685161829628373760791083545457573498600656412030353579510452843445377415943924958414311373173951242344875240776

phi = (p - 1) * (q - 1)
print(gcd(e, phi)) # 4
print(gcd(e, p - 1)) # 2
print(gcd(e, q - 1)) # 2

print(gcd(e,(p-1)//2)) # 1
print(gcd(e,(q-1)//2)) # 1

```

在这里，我们注意到 <img src="https://www.zhihu.com/equation?tex=e" alt="e" class="ee_img tr_noresize" eeimg="1"> 同时与 <img src="https://www.zhihu.com/equation?tex=\frac{p-1}{2}" alt="\frac{p-1}{2}" class="ee_img tr_noresize" eeimg="1"> 和 <img src="https://www.zhihu.com/equation?tex=\frac{q-1}{2}" alt="\frac{q-1}{2}" class="ee_img tr_noresize" eeimg="1"> 互素，于是我们能有一个取巧的方法，先转化到对因子的同余式，即


<img src="https://www.zhihu.com/equation?tex=c\equiv\;m^e\;mod\;p\\
" alt="c\equiv\;m^e\;mod\;p\\
" class="ee_img tr_noresize" eeimg="1">


<img src="https://www.zhihu.com/equation?tex=c\equiv\;m^e\;mod\;q\\
" alt="c\equiv\;m^e\;mod\;q\\
" class="ee_img tr_noresize" eeimg="1">

再不妨记


<img src="https://www.zhihu.com/equation?tex=s_p=\frac{p-1}{2}\\
" alt="s_p=\frac{p-1}{2}\\
" class="ee_img tr_noresize" eeimg="1">


<img src="https://www.zhihu.com/equation?tex=s_q=\frac{q-1}{2}\\
" alt="s_q=\frac{q-1}{2}\\
" class="ee_img tr_noresize" eeimg="1">

容易有


<img src="https://www.zhihu.com/equation?tex=(e,s_p)=(e,s_q)=1\\
" alt="(e,s_p)=(e,s_q)=1\\
" class="ee_img tr_noresize" eeimg="1">

那么必然存在


<img src="https://www.zhihu.com/equation?tex=ed_{s_p}\equiv1\;mod\;s_p\\
" alt="ed_{s_p}\equiv1\;mod\;s_p\\
" class="ee_img tr_noresize" eeimg="1">


<img src="https://www.zhihu.com/equation?tex=ed_{s_q}\equiv1\;mod\;s_q\\
" alt="ed_{s_q}\equiv1\;mod\;s_q\\
" class="ee_img tr_noresize" eeimg="1">

写成等式，即


<img src="https://www.zhihu.com/equation?tex=ed_{s_q}=1+ks_q\\
" alt="ed_{s_q}=1+ks_q\\
" class="ee_img tr_noresize" eeimg="1">


<img src="https://www.zhihu.com/equation?tex=ed_{s_p}=1+ks_p\\
" alt="ed_{s_p}=1+ks_p\\
" class="ee_img tr_noresize" eeimg="1">

我们考虑同余式


<img src="https://www.zhihu.com/equation?tex=m^{\phi(p)}\equiv\;m^{p-1}\equiv\;(m^{s_p})^2\equiv1\;mod\;p
" alt="m^{\phi(p)}\equiv\;m^{p-1}\equiv\;(m^{s_p})^2\equiv1\;mod\;p
" class="ee_img tr_noresize" eeimg="1">

这里就初见端倪了，我们做一个换元


<img src="https://www.zhihu.com/equation?tex=x_p=m^{s_p}
" alt="x_p=m^{s_p}
" class="ee_img tr_noresize" eeimg="1">

那不就是二次剩余的判别式吗


<img src="https://www.zhihu.com/equation?tex=m^{s_p}\;mod\;p\;\equiv\;\begin{cases}1\;\;;m^{s_p}是\;p的二次剩余\\-1\;\;;m^{s_p}不是p的二次剩余\end{cases}
" alt="m^{s_p}\;mod\;p\;\equiv\;\begin{cases}1\;\;;m^{s_p}是\;p的二次剩余\\-1\;\;;m^{s_p}不是p的二次剩余\end{cases}
" class="ee_img tr_noresize" eeimg="1">

注意到这里我们得到了重要的模 <img src="https://www.zhihu.com/equation?tex=p" alt="p" class="ee_img tr_noresize" eeimg="1"> 式子，回看解密过程


<img src="https://www.zhihu.com/equation?tex=c^{d_{s_p}}\equiv\;m^{ed_{s_p}}\equiv\;m^{1+k_{s_p}}\equiv\;m*(m^{s_p})^k
" alt="c^{d_{s_p}}\equiv\;m^{ed_{s_p}}\equiv\;m^{1+k_{s_p}}\equiv\;m*(m^{s_p})^k
" class="ee_img tr_noresize" eeimg="1">

也就是


<img src="https://www.zhihu.com/equation?tex=c^{d_{s_p}}\equiv\;\pm\;m\;mod\;p
" alt="c^{d_{s_p}}\equiv\;\pm\;m\;mod\;p
" class="ee_img tr_noresize" eeimg="1">

也就是说我们有


<img src="https://www.zhihu.com/equation?tex=m_1\equiv\;m\;\equiv\;\pm\;c^{d_{s_p}}\;mod\;p
" alt="m_1\equiv\;m\;\equiv\;\pm\;c^{d_{s_p}}\;mod\;p
" class="ee_img tr_noresize" eeimg="1">

回到了熟悉的地方，再构造 CRT 就可以解出 <img src="https://www.zhihu.com/equation?tex=m" alt="m" class="ee_img tr_noresize" eeimg="1"> 了

那么这里如果再改成


<img src="https://www.zhihu.com/equation?tex=(e,p-1)=(e,q-1)=4,8,16...
" alt="(e,p-1)=(e,q-1)=4,8,16...
" class="ee_img tr_noresize" eeimg="1">

呢，思路还是考虑这个式子


<img src="https://www.zhihu.com/equation?tex=(m^{\frac{p-1}{2^i}})^{2^i}\equiv1\;mod\;p
" alt="(m^{\frac{p-1}{2^i}})^{2^i}\equiv1\;mod\;p
" class="ee_img tr_noresize" eeimg="1">

可以考虑去计算 <img src="https://www.zhihu.com/equation?tex=2^i" alt="2^i" class="ee_img tr_noresize" eeimg="1"> 次的本原单位根，但考虑到虚数的引用带来的一系列不确定性，不再过度深入。

那么如果 <img src="https://www.zhihu.com/equation?tex=e=65537" alt="e=65537" class="ee_img tr_noresize" eeimg="1"> 这种大素数呢？往往会伴随另外一个条件，是 <img src="https://www.zhihu.com/equation?tex=e\;|\;\phi(N)" alt="e\;|\;\phi(N)" class="ee_img tr_noresize" eeimg="1"> ,在这种情况下，我们可以使用 AMM 算法来找到一个满足条件的 <img src="https://www.zhihu.com/equation?tex=m" alt="m" class="ee_img tr_noresize" eeimg="1"> 

## AMM 算法

我们先从上面的 RSA 题目中抽离出来，来看看整数域开根下的一般问题，我们面对的是同余式


<img src="https://www.zhihu.com/equation?tex=m^e\;\equiv\;b\;mod\;p
" alt="m^e\;\equiv\;b\;mod\;p
" class="ee_img tr_noresize" eeimg="1">

我们往往已知 <img src="https://www.zhihu.com/equation?tex=b,p,e" alt="b,p,e" class="ee_img tr_noresize" eeimg="1"> 且 <img src="https://www.zhihu.com/equation?tex=p" alt="p" class="ee_img tr_noresize" eeimg="1"> 是一个素数，我们想要找出一个满足方程的 <img src="https://www.zhihu.com/equation?tex=m" alt="m" class="ee_img tr_noresize" eeimg="1"> ，从简单出发，我们先讨论 <img src="https://www.zhihu.com/equation?tex=e=2" alt="e=2" class="ee_img tr_noresize" eeimg="1"> 的情况

### e=2 时

也就是处理方程


<img src="https://www.zhihu.com/equation?tex=m^2\;\equiv\;b\;mod\;p
" alt="m^2\;\equiv\;b\;mod\;p
" class="ee_img tr_noresize" eeimg="1">

因为 <img src="https://www.zhihu.com/equation?tex=p" alt="p" class="ee_img tr_noresize" eeimg="1"> 是素数，我们可以这样来设


<img src="https://www.zhihu.com/equation?tex=p-1=2^t*s
" alt="p-1=2^t*s
" class="ee_img tr_noresize" eeimg="1">

其中 <img src="https://www.zhihu.com/equation?tex=t>0,s为奇数" alt="t>0,s为奇数" class="ee_img tr_noresize" eeimg="1"> 

那么我们知道，对于所有的模 <img src="https://www.zhihu.com/equation?tex=p" alt="p" class="ee_img tr_noresize" eeimg="1"> 的二次剩余数 <img src="https://www.zhihu.com/equation?tex=x" alt="x" class="ee_img tr_noresize" eeimg="1">  （**这里 <img src="https://www.zhihu.com/equation?tex=b" alt="b" class="ee_img tr_noresize" eeimg="1"> 就是一个模 <img src="https://www.zhihu.com/equation?tex=p" alt="p" class="ee_img tr_noresize" eeimg="1"> 的二次剩余**）有


<img src="https://www.zhihu.com/equation?tex=x^{\frac{p-1}{2}}\equiv1\;mod\;p
" alt="x^{\frac{p-1}{2}}\equiv1\;mod\;p
" class="ee_img tr_noresize" eeimg="1">

代入上面的式子，可以得到


<img src="https://www.zhihu.com/equation?tex=x^{s*2^{t-1}}\equiv1\;mod\;p
" alt="x^{s*2^{t-1}}\equiv1\;mod\;p
" class="ee_img tr_noresize" eeimg="1">

再次从简单出发，如果 <img src="https://www.zhihu.com/equation?tex=t=1" alt="t=1" class="ee_img tr_noresize" eeimg="1"> 

那么直接有


<img src="https://www.zhihu.com/equation?tex=x^s\equiv1\;mod\;p
" alt="x^s\equiv1\;mod\;p
" class="ee_img tr_noresize" eeimg="1">

两边同乘一个 <img src="https://www.zhihu.com/equation?tex=x" alt="x" class="ee_img tr_noresize" eeimg="1">  ，又 <img src="https://www.zhihu.com/equation?tex=(x,p)=1" alt="(x,p)=1" class="ee_img tr_noresize" eeimg="1"> ，直接开方，有


<img src="https://www.zhihu.com/equation?tex=x^{\frac{s+1}{2}}\equiv\;x^{\frac{1}{2}}\;mod\;p
" alt="x^{\frac{s+1}{2}}\equiv\;x^{\frac{1}{2}}\;mod\;p
" class="ee_img tr_noresize" eeimg="1">

前面强调了 <img src="https://www.zhihu.com/equation?tex=b" alt="b" class="ee_img tr_noresize" eeimg="1"> 就是这样的一个二次剩余，也就是说，我们代入 <img src="https://www.zhihu.com/equation?tex=x=b" alt="x=b" class="ee_img tr_noresize" eeimg="1"> ，这个方程是成立的，所以说


<img src="https://www.zhihu.com/equation?tex=b^{\frac{s+1}{2}}\equiv\;b^{\frac{1}{2}}\;mod\;p
" alt="b^{\frac{s+1}{2}}\equiv\;b^{\frac{1}{2}}\;mod\;p
" class="ee_img tr_noresize" eeimg="1">

这个方程两边平方后，就是我们非常熟悉的


<img src="https://www.zhihu.com/equation?tex=b^{s+1}\;\equiv\;b\;mod\;p
" alt="b^{s+1}\;\equiv\;b\;mod\;p
" class="ee_img tr_noresize" eeimg="1">

所以说，我们就找到了一个符合要求的 <img src="https://www.zhihu.com/equation?tex=m=b^{\frac{s+1}{2}}" alt="m=b^{\frac{s+1}{2}}" class="ee_img tr_noresize" eeimg="1"> 

那么，如果 <img src="https://www.zhihu.com/equation?tex=t>1" alt="t>1" class="ee_img tr_noresize" eeimg="1"> 呢？

我们知道对于所有的模 <img src="https://www.zhihu.com/equation?tex=p" alt="p" class="ee_img tr_noresize" eeimg="1"> 的非二次剩余 <img src="https://www.zhihu.com/equation?tex=y" alt="y" class="ee_img tr_noresize" eeimg="1"> ,有


<img src="https://www.zhihu.com/equation?tex=y^{\frac{p-1}{2}}\equiv\;y^{s*2^{t-1}}\equiv-1\;mod\;p
" alt="y^{\frac{p-1}{2}}\equiv\;y^{s*2^{t-1}}\equiv-1\;mod\;p
" class="ee_img tr_noresize" eeimg="1">

我们不难得到


<img src="https://www.zhihu.com/equation?tex=(x^{s*2^{t-2}})^2\equiv1\;mod\;p
" alt="(x^{s*2^{t-2}})^2\equiv1\;mod\;p
" class="ee_img tr_noresize" eeimg="1">

开方，能得到两种结果，即


<img src="https://www.zhihu.com/equation?tex=x^{s*2^{t-2}}\equiv1\;mod\;p\\
" alt="x^{s*2^{t-2}}\equiv1\;mod\;p\\
" class="ee_img tr_noresize" eeimg="1">


<img src="https://www.zhihu.com/equation?tex=x^{s*2^{t-2}}\equiv-1\;mod\;p
" alt="x^{s*2^{t-2}}\equiv-1\;mod\;p
" class="ee_img tr_noresize" eeimg="1">

为了避免负数的出现导致开根开出虚数，我们乘上一个非二次剩余，即


<img src="https://www.zhihu.com/equation?tex=x^{s*2^{t-2}}\;\;*\;\;y^{s*2^{t-1}\;*\;k}\;\;\;\;\equiv1\;mod\;p
" alt="x^{s*2^{t-2}}\;\;*\;\;y^{s*2^{t-1}\;*\;k}\;\;\;\;\equiv1\;mod\;p
" class="ee_img tr_noresize" eeimg="1">

这里用 <img src="https://www.zhihu.com/equation?tex=k" alt="k" class="ee_img tr_noresize" eeimg="1"> 来记录是否为负， <img src="https://www.zhihu.com/equation?tex=y" alt="y" class="ee_img tr_noresize" eeimg="1"> 是自己生成的

注意到我们这里 <img src="https://www.zhihu.com/equation?tex=x" alt="x" class="ee_img tr_noresize" eeimg="1"> 实际上就是 <img src="https://www.zhihu.com/equation?tex=b" alt="b" class="ee_img tr_noresize" eeimg="1"> ，而 <img src="https://www.zhihu.com/equation?tex=s,t" alt="s,t" class="ee_img tr_noresize" eeimg="1"> 都是已知的，在代码中我们可以直接算这个 <img src="https://www.zhihu.com/equation?tex=x^{s*2^{t-2}}\;mod\;p" alt="x^{s*2^{t-2}}\;mod\;p" class="ee_img tr_noresize" eeimg="1"> ，如果是 1，就令 <img src="https://www.zhihu.com/equation?tex=k=0" alt="k=0" class="ee_img tr_noresize" eeimg="1"> ，否则为 1

同余式右边为 1 后继续开方，即


<img src="https://www.zhihu.com/equation?tex=x^{s*2^{t-3}}\;\;*\;\;y^{s*2^{t-2}*k_1}\;\;*\;\;y^{s*2^{t-1}*k_2}\;\equiv\;1\;mod\;p
" alt="x^{s*2^{t-3}}\;\;*\;\;y^{s*2^{t-2}*k_1}\;\;*\;\;y^{s*2^{t-1}*k_2}\;\equiv\;1\;mod\;p
" class="ee_img tr_noresize" eeimg="1">

确定 <img src="https://www.zhihu.com/equation?tex=k_2" alt="k_2" class="ee_img tr_noresize" eeimg="1"> 的方法和上面一样，我们一直开方，直到 <img src="https://www.zhihu.com/equation?tex=x" alt="x" class="ee_img tr_noresize" eeimg="1"> 的指数幂中不含 <img src="https://www.zhihu.com/equation?tex=t" alt="t" class="ee_img tr_noresize" eeimg="1"> ,会得到形似如下的式子


<img src="https://www.zhihu.com/equation?tex=x^{s}\;\;*\;\;y^{s*(k_1+2k_2+4k_3+...+2^{t-2}k_{t-1})}\;\equiv\;1\;mod\;p
" alt="x^{s}\;\;*\;\;y^{s*(k_1+2k_2+4k_3+...+2^{t-2}k_{t-1})}\;\equiv\;1\;mod\;p
" class="ee_img tr_noresize" eeimg="1">

两边同乘一个 <img src="https://www.zhihu.com/equation?tex=x" alt="x" class="ee_img tr_noresize" eeimg="1"> ，再开方就得到了我们需要的 <img src="https://www.zhihu.com/equation?tex=m" alt="m" class="ee_img tr_noresize" eeimg="1"> 


<img src="https://www.zhihu.com/equation?tex=m\equiv\;b^{\frac{1}{2}}\;\equiv\;b^{\frac{s+1}{2}}\;\;*\;\;y^{\frac{1}{2}s*(k_1+2k_2+4k_3+...+2^{t-2}k_{t-1})}\;mod\;p
" alt="m\equiv\;b^{\frac{1}{2}}\;\equiv\;b^{\frac{s+1}{2}}\;\;*\;\;y^{\frac{1}{2}s*(k_1+2k_2+4k_3+...+2^{t-2}k_{t-1})}\;mod\;p
" class="ee_img tr_noresize" eeimg="1">

这里的核心思路就是消去 <img src="https://www.zhihu.com/equation?tex=t" alt="t" class="ee_img tr_noresize" eeimg="1"> ，构造


<img src="https://www.zhihu.com/equation?tex=vb^{u}\equiv\;1\;mod\;p
" alt="vb^{u}\equiv\;1\;mod\;p
" class="ee_img tr_noresize" eeimg="1">

的代数式

这里 AMM 找到的只是一个满足要求的解，我们知道在 <img src="https://www.zhihu.com/equation?tex=e=2" alt="e=2" class="ee_img tr_noresize" eeimg="1"> 的情况下，符合条件的 <img src="https://www.zhihu.com/equation?tex=m" alt="m" class="ee_img tr_noresize" eeimg="1"> 有两个，为了找到另一个解，**也就是描述在模 <img src="https://www.zhihu.com/equation?tex=p" alt="p" class="ee_img tr_noresize" eeimg="1"> 下的 <img src="https://www.zhihu.com/equation?tex=-1" alt="-1" class="ee_img tr_noresize" eeimg="1"> **,我们使用如下方法，任意找到一个 <img src="https://www.zhihu.com/equation?tex=h" alt="h" class="ee_img tr_noresize" eeimg="1"> ，使得它不是模 <img src="https://www.zhihu.com/equation?tex=p" alt="p" class="ee_img tr_noresize" eeimg="1"> 的二次剩余，那么有


<img src="https://www.zhihu.com/equation?tex=h^{\frac{p-1}{2}}\equiv\;-1\;mod\;p\\
" alt="h^{\frac{p-1}{2}}\equiv\;-1\;mod\;p\\
" class="ee_img tr_noresize" eeimg="1">


<img src="https://www.zhihu.com/equation?tex=m*h^{\frac{p-1}{2}}\equiv\;-m\;mod\;p\\
" alt="m*h^{\frac{p-1}{2}}\equiv\;-m\;mod\;p\\
" class="ee_img tr_noresize" eeimg="1">


<img src="https://www.zhihu.com/equation?tex=(m*h^{\frac{p-1}{2}})^2\equiv\;(-m)^2\equiv\;b\;mod\;p\\
" alt="(m*h^{\frac{p-1}{2}})^2\equiv\;(-m)^2\equiv\;b\;mod\;p\\
" class="ee_img tr_noresize" eeimg="1">

也就是说， <img src="https://www.zhihu.com/equation?tex=m" alt="m" class="ee_img tr_noresize" eeimg="1"> , <img src="https://www.zhihu.com/equation?tex=m*h^{\frac{p-1}{2}}" alt="m*h^{\frac{p-1}{2}}" class="ee_img tr_noresize" eeimg="1"> 都是满足要求的解，在 <img src="https://www.zhihu.com/equation?tex=GF(p)" alt="GF(p)" class="ee_img tr_noresize" eeimg="1"> 下

### e 更大的时候

我们现在要考虑的是


<img src="https://www.zhihu.com/equation?tex=m^e\equiv\;b\;mod\;p
" alt="m^e\equiv\;b\;mod\;p
" class="ee_img tr_noresize" eeimg="1">

这里的问题是从二次剩余转到 <img src="https://www.zhihu.com/equation?tex=e" alt="e" class="ee_img tr_noresize" eeimg="1"> 次剩余，这里我们需要条件 <img src="https://www.zhihu.com/equation?tex=e\;|\;p-1" alt="e\;|\;p-1" class="ee_img tr_noresize" eeimg="1"> ,设


<img src="https://www.zhihu.com/equation?tex=p-1=e^t*s
" alt="p-1=e^t*s
" class="ee_img tr_noresize" eeimg="1">

那么对我们要求的 <img src="https://www.zhihu.com/equation?tex=x" alt="x" class="ee_img tr_noresize" eeimg="1"> ，有


<img src="https://www.zhihu.com/equation?tex=(x^e)^{\frac{p-1}{e}}\equiv\;c^{\frac{p-1}{e}}\equiv\;c^{s*e^{t-1}}\;\equiv1\;mod\;p\\
" alt="(x^e)^{\frac{p-1}{e}}\equiv\;c^{\frac{p-1}{e}}\equiv\;c^{s*e^{t-1}}\;\equiv1\;mod\;p\\
" class="ee_img tr_noresize" eeimg="1">

还是从简单出发，如果 <img src="https://www.zhihu.com/equation?tex=t=1" alt="t=1" class="ee_img tr_noresize" eeimg="1"> 

即


<img src="https://www.zhihu.com/equation?tex=c^{s}\equiv\;1\;mod\;p
" alt="c^{s}\equiv\;1\;mod\;p
" class="ee_img tr_noresize" eeimg="1">

此时如果 <img src="https://www.zhihu.com/equation?tex=(s,e)=1" alt="(s,e)=1" class="ee_img tr_noresize" eeimg="1"> ，那么可以参考 RSA 的解法


<img src="https://www.zhihu.com/equation?tex=ed\equiv1\;mod\;s\;\;;\;\;ed-1=ks
" alt="ed\equiv1\;mod\;s\;\;;\;\;ed-1=ks
" class="ee_img tr_noresize" eeimg="1">

代入，


<img src="https://www.zhihu.com/equation?tex=c^{ed}\equiv\;(c^d)^e\;\equiv\;c\;mod\;p
" alt="c^{ed}\equiv\;(c^d)^e\;\equiv\;c\;mod\;p
" class="ee_img tr_noresize" eeimg="1">

可见这里 <img src="https://www.zhihu.com/equation?tex=m\equiv\;c^d\;mod\;p" alt="m\equiv\;c^d\;mod\;p" class="ee_img tr_noresize" eeimg="1"> 就是一个满足要求的解

如果 <img src="https://www.zhihu.com/equation?tex=(s,e)\neq1" alt="(s,e)\neq1" class="ee_img tr_noresize" eeimg="1"> 可以考虑离散对数的方法

如果 <img src="https://www.zhihu.com/equation?tex=t>1" alt="t>1" class="ee_img tr_noresize" eeimg="1"> 

我们站在高一点的视角下一个结论


<img src="https://www.zhihu.com/equation?tex=AMM算法就是在模p整数域中构造一个e阶子群来开根
" alt="AMM算法就是在模p整数域中构造一个e阶子群来开根
" class="ee_img tr_noresize" eeimg="1">

先看要讨论的式子


<img src="https://www.zhihu.com/equation?tex=m^e\equiv\;c\;mod\;p
" alt="m^e\equiv\;c\;mod\;p
" class="ee_img tr_noresize" eeimg="1">

其中 <img src="https://www.zhihu.com/equation?tex=e\;|\;p" alt="e\;|\;p" class="ee_img tr_noresize" eeimg="1"> 

我们可以令


<img src="https://www.zhihu.com/equation?tex=p-1\;=\;se^{t}
" alt="p-1\;=\;se^{t}
" class="ee_img tr_noresize" eeimg="1">

由欧拉定理,当 <img src="https://www.zhihu.com/equation?tex=(x,p)=1" alt="(x,p)=1" class="ee_img tr_noresize" eeimg="1"> 


<img src="https://www.zhihu.com/equation?tex=x^{p-1}\equiv\;1\;mod\;p
" alt="x^{p-1}\equiv\;1\;mod\;p
" class="ee_img tr_noresize" eeimg="1">

这里可以视为一个生成元为 <img src="https://www.zhihu.com/equation?tex=x" alt="x" class="ee_img tr_noresize" eeimg="1"> ，阶为 <img src="https://www.zhihu.com/equation?tex=p" alt="p" class="ee_img tr_noresize" eeimg="1"> 的模 <img src="https://www.zhihu.com/equation?tex=p" alt="p" class="ee_img tr_noresize" eeimg="1"> 子群

那么稍微变形一下


<img src="https://www.zhihu.com/equation?tex=(x^{\frac{p-1}{e}})^e\equiv\;1\;mod\;p
" alt="(x^{\frac{p-1}{e}})^e\equiv\;1\;mod\;p
" class="ee_img tr_noresize" eeimg="1">

令


<img src="https://www.zhihu.com/equation?tex=g=x^{\frac{p-1}{e}}
" alt="g=x^{\frac{p-1}{e}}
" class="ee_img tr_noresize" eeimg="1">

那么我们就构造了一个模 <img src="https://www.zhihu.com/equation?tex=p" alt="p" class="ee_img tr_noresize" eeimg="1"> 下，生成元为 <img src="https://www.zhihu.com/equation?tex=g" alt="g" class="ee_img tr_noresize" eeimg="1"> 的 <img src="https://www.zhihu.com/equation?tex=e" alt="e" class="ee_img tr_noresize" eeimg="1"> 阶子群 <img src="https://www.zhihu.com/equation?tex=G" alt="G" class="ee_img tr_noresize" eeimg="1"> 


<img src="https://www.zhihu.com/equation?tex=G=\left\{1,g,g^2,...,g^{e-1}\right\}
" alt="G=\left\{1,g,g^2,...,g^{e-1}\right\}
" class="ee_img tr_noresize" eeimg="1">

特别注意 <img src="https://www.zhihu.com/equation?tex=g" alt="g" class="ee_img tr_noresize" eeimg="1"> 不能是模 <img src="https://www.zhihu.com/equation?tex=p" alt="p" class="ee_img tr_noresize" eeimg="1"> 的 <img src="https://www.zhihu.com/equation?tex=e" alt="e" class="ee_img tr_noresize" eeimg="1"> 次剩余，也就是


<img src="https://www.zhihu.com/equation?tex=g^{\frac{p-1}{e}}\;\not\equiv\;\;1\;mod\;p
" alt="g^{\frac{p-1}{e}}\;\not\equiv\;\;1\;mod\;p
" class="ee_img tr_noresize" eeimg="1">

否则 <img src="https://www.zhihu.com/equation?tex=g" alt="g" class="ee_img tr_noresize" eeimg="1"> 就是 1 开 <img src="https://www.zhihu.com/equation?tex=e" alt="e" class="ee_img tr_noresize" eeimg="1"> 根开出来的一个结果，你拿 <img src="https://www.zhihu.com/equation?tex=g" alt="g" class="ee_img tr_noresize" eeimg="1"> 生成的群就无法包含所有可能的根

接下来我们说明 <img src="https://www.zhihu.com/equation?tex=G" alt="G" class="ee_img tr_noresize" eeimg="1"> 中元素进行 <img src="https://www.zhihu.com/equation?tex=e" alt="e" class="ee_img tr_noresize" eeimg="1"> 次幂后模 <img src="https://www.zhihu.com/equation?tex=p" alt="p" class="ee_img tr_noresize" eeimg="1"> 为 1


<img src="https://www.zhihu.com/equation?tex=g_i^{e}\equiv\;(g^{e})^i\equiv1^i\;\equiv\;1\;mod\;p
" alt="g_i^{e}\equiv\;(g^{e})^i\equiv1^i\;\equiv\;1\;mod\;p
" class="ee_img tr_noresize" eeimg="1">

那么也就是说，在模 <img src="https://www.zhihu.com/equation?tex=p" alt="p" class="ee_img tr_noresize" eeimg="1"> 群下对 1 开 <img src="https://www.zhihu.com/equation?tex=e" alt="e" class="ee_img tr_noresize" eeimg="1"> 次方根的结果都会落在群 <img src="https://www.zhihu.com/equation?tex=G" alt="G" class="ee_img tr_noresize" eeimg="1"> 中

知道这个结论，我们对上式做一个小变形


<img src="https://www.zhihu.com/equation?tex=(x^{s*e^{t-1}})^e\equiv1\;mod\;p
" alt="(x^{s*e^{t-1}})^e\equiv1\;mod\;p
" class="ee_img tr_noresize" eeimg="1">

再


<img src="https://www.zhihu.com/equation?tex=(x^e)^{s*e^{t-1}}\equiv(c)^{s*e^{t-1}}\equiv1\;mod\;p
" alt="(x^e)^{s*e^{t-1}}\equiv(c)^{s*e^{t-1}}\equiv1\;mod\;p
" class="ee_img tr_noresize" eeimg="1">

 <img src="https://www.zhihu.com/equation?tex=c,s,t,e,p" alt="c,s,t,e,p" class="ee_img tr_noresize" eeimg="1"> 我们都知道，仿照上面的思路，先对


<img src="https://www.zhihu.com/equation?tex=(c)^{s*e^{t-1}}
" alt="(c)^{s*e^{t-1}}
" class="ee_img tr_noresize" eeimg="1">

开 <img src="https://www.zhihu.com/equation?tex=e" alt="e" class="ee_img tr_noresize" eeimg="1"> 次方根，就能得到


<img src="https://www.zhihu.com/equation?tex=(c)^{s*e^{t-2}}
" alt="(c)^{s*e^{t-2}}
" class="ee_img tr_noresize" eeimg="1">

我们再直接计算这个元素的值，然后在这个群 <img src="https://www.zhihu.com/equation?tex=G" alt="G" class="ee_img tr_noresize" eeimg="1"> 中找到逆元

假设


<img src="https://www.zhihu.com/equation?tex=(c)^{s*e^{t-2}}\equiv\;g^i\;mod\;p
" alt="(c)^{s*e^{t-2}}\equiv\;g^i\;mod\;p
" class="ee_img tr_noresize" eeimg="1">

那么逆元就是


<img src="https://www.zhihu.com/equation?tex=k_j\equiv\;g^{e-i}\;mod\;p
" alt="k_j\equiv\;g^{e-i}\;mod\;p
" class="ee_img tr_noresize" eeimg="1">

所以说上面的同余式开根后就直接是


<img src="https://www.zhihu.com/equation?tex=(c)^{s*e^{t-2}}\;\;*\;\;k_j\;\;\equiv\;\;1\;\;mod\;p
" alt="(c)^{s*e^{t-2}}\;\;*\;\;k_j\;\;\equiv\;\;1\;\;mod\;p
" class="ee_img tr_noresize" eeimg="1">

如此往下计算，直到同余式左边只剩下 <img src="https://www.zhihu.com/equation?tex=s" alt="s" class="ee_img tr_noresize" eeimg="1"> 次幂，最后解出来 <img src="https://www.zhihu.com/equation?tex=m" alt="m" class="ee_img tr_noresize" eeimg="1"> 之后还要记得根据这个群 <img src="https://www.zhihu.com/equation?tex=G" alt="G" class="ee_img tr_noresize" eeimg="1"> 的 <img src="https://www.zhihu.com/equation?tex=e" alt="e" class="ee_img tr_noresize" eeimg="1"> 个单位根对应出 <img src="https://www.zhihu.com/equation?tex=e" alt="e" class="ee_img tr_noresize" eeimg="1"> 组解，当 <img src="https://www.zhihu.com/equation?tex=(e,s)=1" alt="(e,s)=1" class="ee_img tr_noresize" eeimg="1"> 的时候，我们来操作一下，就是

### 分解 p-1


<img src="https://www.zhihu.com/equation?tex=p-1=se^{t}
" alt="p-1=se^{t}
" class="ee_img tr_noresize" eeimg="1">

### 计算逆元 d，找到初始解


<img src="https://www.zhihu.com/equation?tex=d\equiv\;e^{-1}\;mod\;s
" alt="d\equiv\;e^{-1}\;mod\;s
" class="ee_img tr_noresize" eeimg="1">


<img src="https://www.zhihu.com/equation?tex=m_0\;\equiv\;c^{d}\;mod\;p
" alt="m_0\;\equiv\;c^{d}\;mod\;p
" class="ee_img tr_noresize" eeimg="1">

### 逐步上升

先找到生成元 <img src="https://www.zhihu.com/equation?tex=g" alt="g" class="ee_img tr_noresize" eeimg="1"> 


<img src="https://www.zhihu.com/equation?tex=g^{\frac{p-1}{e}}\;\not\equiv\;\;1\;mod\;p
" alt="g^{\frac{p-1}{e}}\;\not\equiv\;\;1\;mod\;p
" class="ee_img tr_noresize" eeimg="1">

再定义一些初始变量


<img src="https://www.zhihu.com/equation?tex=a\equiv\;g^{e^{t-1}}\;mod\;s
" alt="a\equiv\;g^{e^{t-1}}\;mod\;s
" class="ee_img tr_noresize" eeimg="1">


<img src="https://www.zhihu.com/equation?tex=b\;\equiv\;c^{ed-1}\;\equiv\;\frac{(c^d)^e}{c}\;\equiv\;\frac{m_0^{e}}{c}\;mod\;p
" alt="b\;\equiv\;c^{ed-1}\;\equiv\;\frac{(c^d)^e}{c}\;\equiv\;\frac{m_0^{e}}{c}\;mod\;p
" class="ee_img tr_noresize" eeimg="1">


<img src="https://www.zhihu.com/equation?tex=z\;\equiv\;g^s\;mod\;p
" alt="z\;\equiv\;g^s\;mod\;p
" class="ee_img tr_noresize" eeimg="1">


<img src="https://www.zhihu.com/equation?tex=h=1
" alt="h=1
" class="ee_img tr_noresize" eeimg="1">

可知


<img src="https://www.zhihu.com/equation?tex=z^{e^{t}}\equiv\;g^{s*e^{t}}\;\equiv\;g^{p-1}\;\equiv\;1\;mod\;p
" alt="z^{e^{t}}\equiv\;g^{s*e^{t}}\;\equiv\;g^{p-1}\;\equiv\;1\;mod\;p
" class="ee_img tr_noresize" eeimg="1">

再在每一次迭代的过程中，计算


<img src="https://www.zhihu.com/equation?tex=d_i\equiv\;b^{e^{t-1-i}}\;mod\;p
" alt="d_i\equiv\;b^{e^{t-1-i}}\;mod\;p
" class="ee_img tr_noresize" eeimg="1">

如果


<img src="https://www.zhihu.com/equation?tex=d_i=1
" alt="d_i=1
" class="ee_img tr_noresize" eeimg="1">

说明这次开方的结果开出来就是 <img src="https://www.zhihu.com/equation?tex=e" alt="e" class="ee_img tr_noresize" eeimg="1"> 次剩余根，无需调整，否则要进行修改，并同时对 <img src="https://www.zhihu.com/equation?tex=b,h,z" alt="b,h,z" class="ee_img tr_noresize" eeimg="1"> 进行修正

最后对得到的根乘上 <img src="https://www.zhihu.com/equation?tex=e" alt="e" class="ee_img tr_noresize" eeimg="1"> 个单位根，得到解

```python
# sage
def AM1M(residue, e, p):
    """
    返回 x^e ≡ residue mod p 的所有解
    """
    if (p-1) % e != 0:
        raise ValueError(f"e={e} 不能整除 p-1={p-1}，AMM 不适用")

    # 分解 p-1 = e^t * s
    t = valuation(p - 1, e)
    s = (p - 1) // (e^t)

    # 计算 d = e^{-1} mod s
    d = inverse_mod(e, s)

    # 简单情况：t=1
    if t == 1:
        x = power_mod(residue, d, p)
        F = GF(p)
        zeta = F.zeta(e)
        return [ (x * zeta**i) % p for i in range(e) ]


    # 寻找 e 次非剩余（生成元）
    F = GF(p)
    g = F.multiplicative_generator()
    noresidue = g
    while (noresidue^((p-1)//e)) == 1:
        noresidue *= g

    # 计算单位根和辅助变量
    a = power_mod(noresidue, e^(t-1) * s, p)
    b = power_mod(residue, e * d - 1, p)
    c = power_mod(noresidue, s, p)
    h = 1

    # Hensel 提升
    for i in range(1, t):
        d_i = power_mod(b, e^(t-1-i), p)
        if d_i == 1:
            j = 0
        else:
            j = -discrete_log(d_i, a)
        b = (power_mod(c, e, p)^j * b) % p
        h = (power_mod(c, j, p) * h) % p
        c = power_mod(c, e, p)

    x0 = (power_mod(residue, d, p) * h) % p
    ω = F.zeta(e)  # e 阶单位根
    return [ (x0 * ω^i) % p for i in range(e) ]  # 返回所有解

```

这里能得到所有模 p 下的解，再整合起来打 CRT 就解决了

来个实战例子

**NCTF2019--EzRSA**

```py
e = 0x1337
p = 199138677823743837339927520157607820029746574557746549094921488292877226509198315016018919385259781238148402833316033634968163276198999279327827901879426429664674358844084491830543271625147280950273934405879341438429171453002453838897458102128836690385604150324972907981960626767679153125735677417397078196059
q = 112213695905472142415221444515326532320352429478341683352811183503269676555434601229013679319423878238944956830244386653674413411658696751173844443394608246716053086226910581400528167848306119179879115809778793093611381764939789057524575349501163689452810148280625226541609383166347879832134495444706697124741
n = p * q
c = 10562302690541901187975815594605242014385201583329309191736952454310803387032252007244962585846519762051885640856082157060593829013572592812958261432327975138581784360302599265408134332094134880789013207382277849503344042487389850373487656200657856862096900860792273206447552132458430989534820256156021128891296387414689693952047302604774923411425863612316726417214819110981605912408620996068520823370069362751149060142640529571400977787330956486849449005402750224992048562898004309319577192693315658275912449198365737965570035264841782399978307388920681068646219895287752359564029778568376881425070363592696751183359

phi = (p - 1) * (q - 1)
print(gcd(e, phi)) #4919
print(gcd(e, p - 1)) #4919
print(gcd(e, q - 1)) # 4919
print(int(e)) #4919

```

exp 这里使用 python 多进程加快运算

```python
from Crypto.Util.number import *
import itertools
from multiprocessing import Pool, cpu_count
from tqdm import tqdm
from sage.all import *

e = 0x1337
p = 199138677823743837339927520157607820029746574557746549094921488292877226509198315016018919385259781238148402833316033634968163276198999279327827901879426429664674358844084491830543271625147280950273934405879341438429171453002453838897458102128836690385604150324972907981960626767679153125735677417397078196059
q = 112213695905472142415221444515326532320352429478341683352811183503269676555434601229013679319423878238944956830244386653674413411658696751173844443394608246716053086226910581400528167848306119179879115809778793093611381764939789057524575349501163689452810148280625226541609383166347879832134495444706697124741
n = p * q
c = 10562302690541901187975815594605242014385201583329309191736952454310803387032252007244962585846519762051885640856082157060593829013572592812958261432327975138581784360302599265408134332094134880789013207382277849503344042487389850373487656200657856862096900860792273206447552132458430989534820256156021128891296387414689693952047302604774923411425863612316726417214819110981605912408620996068520823370069362751149060142640529571400977787330956486849449005402750224992048562898004309319577192693315658275912449198365737965570035264841782399978307388920681068646219895287752359564029778568376881425070363592696751183359

cp = c % p
cq = c % q

inv_p = pow(p, -1, q)
inv_q = pow(q, -1, p)

def AMM(residue, e, p):
    if (p-1) % e != 0:
        raise ValueError(f"e={e} does not divide p-1={p-1}")

    t = valuation(p - 1, e)
    s = (p - 1) // (e**t)

    d = inverse_mod(e, s)

    if t == 1:
        x = power_mod(residue, d, p)
        F = GF(p)
        zeta = F.zeta(e)
        return [ (x * zeta**i) % p for i in range(e) ]

    F = GF(p)
    g = F.multiplicative_generator()
    noresidue = g
    while (noresidue**((p-1)//e)) == 1:
        noresidue *= g

    a = power_mod(noresidue, e**(t-1) * s, p)
    b = power_mod(residue, e * d - 1, p)
    c = power_mod(noresidue, s, p)
    h = 1

    for i in range(1, t):
        d_i = power_mod(b, e**(t-1-i), p)
        if d_i == 1:
            j = 0
        else:
            j = -discrete_log(d_i, a)
        b = (power_mod(c, e, p)**j * b) % p
        h = (power_mod(c, j, p) * h) % p
        c = power_mod(c, e, p)

    x0 = (power_mod(residue, d, p) * h) % p
    ω = F.zeta(e)
    return [ (x0 * ω**i) % p for i in range(e) ]

def compute_crt(args):
    m1, m2 = args
    m1_int = int(m1)  # Convert to Python int
    m2_int = int(m2)  # Convert to Python int
    x = (m1_int * q * inv_q + m2_int * p * inv_p) % n
    return x

def find_flag():
    resp = AMM(cp, e, p)
    resq = AMM(cq, e, q)

    print(f"[+] Solutions in GF(p): {len(resp)}")
    print(f"[+] Solutions in GF(q): {len(resq)}")
    print(f"[+] Total combinations: {len(resp) * len(resq)}")

    with Pool(cpu_count()) as pool:
        combinations = itertools.product(resp, resq)
        for x in tqdm(
            pool.imap_unordered(compute_crt, combinations, chunksize=10000),
            total=len(resp) * len(resq),
            desc="Brute-forcing CRT"
        ):
            flag = long_to_bytes(x)
            if b'NCTF{' in flag:
                print(f"\n[+] Flag: {flag.decode()}")
                pool.terminate()
                return

    print("[-] Flag not found")

if __name__ == "__main__":
    find_flag()

    # 大约跑三分钟

```
