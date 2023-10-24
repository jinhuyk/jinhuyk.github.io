---
layout: post
title:  "소수"
categories: Algorithm math prime
use_math: true
---

정수론 - 소수에 관한 지식 정리

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