---
layout: post
title:  "정수론"
categories: algorithm number-theory
use_math: true
---

Topic : 소수, 유클리드 알고리즘

## 소수
소수는 기본적으로 정수론에서 가장 많이 사용되는 주제이다.
정수 $n$ 이 소수가 아니라면 두 정수의 곱 $a \times b$ 로 나타낼 수 있고, 이때 $a \le \sqrt n$ 또는 $b \le \sqrt n$가 성립한다. 즉 2 이상 $\sqrt n$ 이하의 인수가 반드시 존재한다. 

#### 소수판별
어떤 정수 N 이 소수인지 판별하기 위해서는 N을 2부터 $\sqrt n$까지 나눠보면서 만약 하나도 나누어 떨어지지 않는 경우에 소수라고 판별한다. 다음 코드는 $O(\sqrt n)$ 의 시간복잡도를 가진다.

~~~c++
bool prime(int n ){
    if(n < 2) return false;
    for(int x = 2; x*x <= n;x++){
        if(N%x == 0) return false;
    }
    return true;
}
~~~

#### 소인수분해
위에서 말한 소수판별 알고리즘을 응용하면 N의 소인수들을 vector에 담을 수 있다.
만약 loop 이후에 n이 1보다 크다면 이는 소수라는 것을 알수 있다.
~~~c++
vector<int>factors(int n){
    vector<int> f;
    for(int x =2; x*x <= n; x++){
        while(n%x == 0){
            f.push_back(x);
            n/=x;
        }
    }
    if( n>1) f.push_back(n);
    return f;
}
~~~

#### 에라토스테네스의 체
에라토스테네스의 체는 2 이상 n 이하의 정수 x 가 소수인지를 효율적으로 판별할 수 있게 sieve 라는 배열을 만드는 전처리 알고리즘에 해당한다.
$x$ 가 소수라면 sieve[x] = 0 이다.

알고리즘은 다음처럼 작동한다.
- x가 새로운 소수라면 2x,3x,4x, ... 은 모두 소수가 아님으로 처리한다.
- 전처리 과정을 하면, 소수 판별에 대해서는 $O(1)$ 에 할수 있다.

~~~c++
for (int x = 2; x<= n;x++){
    if(sieve[x]) continue;
    for(int u = 2*x; u <= n ;u+=x){
        sieve[i] = 1;
    }
}
~~~
전처리 시 알고리즘의 안쪽 반복문은 x에 대해 $$ n/x$$ 번 수행하게 되므로, 전체적으로 $O(n \log \log n)$ 만큼의 시간이 걸린다.

## 유클리드 알고리즘
유클리드 알고리즘은 PS나 수학에서 자주 다루게 되는 주제이다.

#### lcm 과 gcd
lcm 이란 $a,b$의 최소 공배수를 의미한다. 이는 $a$와 $b$ 로 모두 나누어 떨어지는 가장 작은 정수를 의미한다.
gcd 란 $a,b$ 모두의 약수 중 가장 큰 정수를 말한다.

lcm과 gcd 는 다음과 같은 관계를 가진다.

$$
lcm(a,b) = (a*b)/gcd(a,b)
$$

lcm을 찾기 위해서는 두 수의 gcd 가 필요하므로 gcd를 구해야한다. 

#### 유클리드 알고리즘

유클리드 알고리즘은 $gcd(a,b)$ 를 효율적으로 계산하는 방법이다.

```cpp
algorithm gcd(int a, int b)

if(b == 0) return a
else gcd(b , a % b)
```

다음처럼 재귀의 형태로 나타낼 수 있는 이유는  x = gcd(a,b) 라 한다면 x는 a와 b의 모두의 약수, a mod b 의 약수 이기도 하다.

다음 gcd 알고리즘의 시간 복잡도는 $O(\log (min(a,b)))$와 같다.

#### 확장 유클리드 알고리즘
확장 유클리드 알고리즘이란 다음 식을 만족하는 정수 x,y 를 구할 수 있다.
$$ ax+by = gcd(a,b)$$

이도 Euclid’s algorithm 을 이용하여 풀수 있는데, 다음처럼 생각할 수 있다.

$$ bx' + (a\mod b)y' = \gcd(a,b)\\ bx' + (a - a/b * b)y' = \gcd(a,b)\\ ay' + b(x' - a/b *y') = \gcd(a,b) $$

다음 처럼 식을 변형 시켜 a,b에 대한 식으로 만들 수 있고, 이는 tuple을 이용하여 다음처럼 코드를 만들 수 있다.

```cpp
tuple<int,int,int> ext_gcd(int a, int b){
    if(b== 0) return {a,1,0};
    auto [g,x,y] = ext_gcd(b,a%b);
    return {g,y, x- a/b *y};
}
```

#### 디오판토스 방정식 (Diophantine equation)
디오판토스 방정식은 $ax+by= c$ 형태의 식에서 x와 y를 구해야 하고 , 모든 값이 정수인 형태를 말한다.

디오판토스 방정식은 특수해를 알면 일반해를 무한히 구할 수 있다.

$$
d = \gcd(a,b) \\
ax' + by' = d\\
a(cx'/d) + b (cy'/d) = c
$$

다음 식을 통해서 특수 해를 구할 수 있다.

해당 식의 $x',y'$는 확장 유클리드 알고리즘을 통해 구할 수 있다.

이후 일반해는 다음을 이용하여 구하면 된다.

$$
(x,y) = ( x_0 + b*t/d , y_0 + a*t/d) \\
$$

이때 t 는 임의의 정수이다.

---
### 관련 문제

#### LCM - BOJ11414

- 두 자연수 A,B 가 주어졌을 때 A+N 과 B+N 의 최소공배수가 최소가 되는 자연수 N 찾기
- $gcd(a+N,b+N) = gcd(a+N, b-a)$
- 위의 식을 이용하면 결국 $B-A$ 의 약수가 gcd가 되는 것을 알 수 있음
- $B-A$ 의 약수를 구한 뒤 lcm을 구해보며 최소인 N을 찾으면 됨
- 약수 들 중 $A$ 와 약수인 경우에는 $N = factor - A \mod factor$ 에서 결국 $factor = N$ 이 됨 이 경우에는 lcm 이 크게 나올 수 밖에 없음

```cpp
#include <iostream>
#include <algorithm>
#include <vector>
#include <deque>

using namespace std;
typedef long long ll;

ll A,B;
int main(void){
    cin >> A >> B;
    if(A == B){
        cout << 1<<"\n";
        return 0;
    }
    if(A > B) swap(A,B);
    
    vector<ll> factor;
    for(int i= 1; i*i <= B-A ;i++){
        if((B-A)%i==0) {
            factor.push_back(i);
            if((B-A)!=i*i) factor.push_back((B-A)/i);
        }
    }
    ll mn = B-A, lcm = 1e18;
    for(auto f : factor){
        if(A % f == 0) continue;
        ll N = f - A%f;
        ll temp = (A+N)*(B+N)/f;
        if(lcm > temp){
            lcm = temp;
            mn = N;
            
        } else if (lcm == temp && mn > N) mn = N;
    }
    cout << mn <<"\n";
    
}
```

