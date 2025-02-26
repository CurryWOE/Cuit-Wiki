字符串映射为整数，$O(1)$ 比较字符串

## 原理
$hash=\sum\limits_{i=1}^n base^{n-i}a_i$

把字符串看成 $base$ 进制数，并模一个大数解决溢出

$0<a_i<base<mod,gcd(base,mod)=1$

例如把只有小写字母的字符串的 $a_i$设为 $s_i-'a'+1$

这些限制都是为了减少冲突可能性

base可以取131或13331，冲突小

大数不可以取 $2^{64}$ （即ull自然溢出），因为会被卡掉

大数最好用大质数，可以取1e9+9

或者使用双模数（即使用两个模数对一个字符串计算两遍哈希值，根据中国剩余定理，这可以使哈希范围扩大到两数乘积

当然可以类似的使用更多模数

```cpp
typedef unsigned long long ull;
const int N=1e6+1;
const ull base=131;
const ull mod=1e9+9;
ull has[N],power[N];
void init()
{
    power[0]=1;
    for(int i=1;i<N;++i)
        power[i]=power[i-1]*base%mod;
}
void Hash(string s)
{
    s=" "+s;
    int len=s.length();
    has[0]=0;
    for(int i=1;i<=len;++i)
        has[i]=(has[i-1]*base+s[i]-'a'+1)%mod;
}
ull getSectionHash(int l,int r)
{
    return (has[r]-has[l-1]*power[r-l+1]%mod)%mod;
}
```
## 判断回文串
原哈希值和reverse哈希值一样
```cpp
ull rev[N];
void Hash(string s)
{
    ...
    has[0]=0;
    rev[0]=0;
    for(int i=1;i<=len;++i)
    {
        has[i]=(has[i-1]*base+s[i]-'a'+1)%mod;
        rev[i]=(rev[i-1]*base+s[len+1-i]-'a'+1)%mod;
    }
}
ull getRevSectionHash(int l,int r)
{
    return (rev[n-l+1]-has[n-r]*power[r-l+1]%mod)%mod;
}
```
