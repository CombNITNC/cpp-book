# 数学的な関数

$y = \sqrt{sin^3 x}$

---

ここでは、C++ 標準ライブラリに入ってる数学的な関数を解説するよ。

理論的な処理をするときは、それなりの頻度で使う。


# cmath

C++ では、`double` (とその他の型) で計算する数学の関数が用意されている。

それらを使うには、`cmath` ファイルを `#include` する必要があるよ。

以下のコードを動かすときは、

```cpp
#include <cmath>
```

を忘れないでね。


## pow

`std::pow(x, y)` のときに `x の y 乗` を計算する。

```cpp
std::cout << "2 ^ 3 = " << std::pow(2, 3)
  << "\n8 ^ 1.2 = " << std::pow(8, 1.2)
  << "\n4 ^ 0.5 = " << std::pow(4, 0.5);
```


## sqrt

引数の平方根 (英語で Square-root を略して sqrt) を計算する。

```cpp
std::cout << "√9 = " << std::sqrt(9)
  << "\n√8 = " << std::sqrt(8)
  << "\n√-1 = " << std::sqrt(-1);
  // sqrt(-1) は Not A Number エラーになる
```


## round / ceil / floor / trunc

小数を整数にする関数 4 つをまとめて紹介。

`round` (日本語で丸める) は四捨五入を計算する。負の数でも 0 から遠い方向に丸める。

```cpp
using std::round;
round(2.0); // 2
round(2.1); // 2
round(2.5); // 3
round(2.9); // 3
round(-2.0); // -2
round(-2.1); // -2
round(-2.5); // -3
round(-2.9); // -3
```

`ceil` (日本語で天井) は天井関数。その数以上の整数にする。

```cpp
using std::ceil;
ceil(2.0); // 2
ceil(2.1); // 3
ceil(2.5); // 3
ceil(2.9); // 3
ceil(-2.0); // -2
ceil(-2.1); // -2
ceil(-2.5); // -2
ceil(-2.9); // -2
```

`floor` (日本語で床) は床関数。その数以下の整数にする。

```cpp
using std::floor;
floor(2.0); // 2
floor(2.1); // 2
floor(2.5); // 2
floor(2.9); // 2
floor(-2.0); // -2
floor(-2.1); // -3
floor(-2.5); // -3
floor(-2.9); // -3
```

`trunc` (truncate は日本語で切り捨て) は切り捨て関数。小数点以下を消す。

```cpp
using std::floor;
floor(2.0); // 2
floor(2.1); // 2
floor(2.5); // 2
floor(2.9); // 2
floor(-2.0); // -2
floor(-2.1); // -2
floor(-2.5); // -2
floor(-2.9); // -2
```

全部覚えなくてもいいけど、小数に 4 種類の丸め方があることを知ってたら幸せになれるかも。


## sin / cos / tan

みんな大好き三角関数。引数の単位はラジアンだから注意。

```cpp
std::sin(3.14); // だいたい 0
std::sin(1.57); // だいたい 1

std::cos(3.14); // だいたい -1
std::cos(1.57); // だいたい 0

std::tan(3.14); // だいたい 0
std::tan(1.57); // だいたい inf
```

他にもいろんな関数がある。日本語のサイトだと [ここ](https://cpprefjp.github.io/reference/cmath.html#trigonometric-functions) とか見るといいかも。


# numeric

`int` のような整数について計算する関数もあるけれど、これらは `numeric` にある。

以下のコードを動かすときは、

```cpp
#include <numeric>
```

を忘れないでね。


## gcd

`gcd` (Greatest Common Divisor の略) は最大公約数を計算する。

```cpp
using std::gcd;
gcd(2, 3); // 1
gcd(6, 12); // 6
gcd(36, 48); // 12
```

## lcm

`lcm` (Least Common Multiple の略) は最小公倍数を計算する。

```cpp
using std::lcm;
lcm(2, 3); // 6
lcm(6, 12); // 12
lcm(36, 48); // 144
```
