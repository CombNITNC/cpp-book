# 最適化

クリア・マインド

---

ここでは、書いたプログラムの実行効率と、その改善に必要な前提知識をやっていくよ。

速いプログラムが書けるようになろう。


## アルゴリズムの優劣

まず簡単な例として、`1` から `n` までの総和を求める関数を考えよう。

言われたとおりに `1` から `n` まで足すのであれば、こうすることになる。

```cpp
int sum_from_1_to(int n) {
  int sum = 0:
  for (int i = 1; i < n; ++i) {
    sum += i;
  }
  return sum;
}
```

でも、「`1` から `n` までの総和 = `(n + 1) * n / 2`」という数学知識を知っていれば、こう書くだけでいい。

```cpp
int sum_from_1_to(int n) {
  // 1 から n までの総和の式 : (n + 1) * n / 2
  return (n + 1) * n / 2;
}
```

上のプログラムは `n` 回ループをしているので、`n` が大きくなるほど計算に時間がかかる。

下のプログラムは `n` がどれだけ大きくなっても、同じ計算しかしないので計算にかかる時間は変わらない。

下のプログラムの方が優れているのは、言うまでもないよね?


## 入力で爆発する計算量

もう少し、入力の大きさがより計算する量に影響を及ぼす例を見ていこう。

`n` 番目の [フィボナッチ数](https://ja.wikipedia.org/wiki/%E3%83%95%E3%82%A3%E3%83%9C%E3%83%8A%E3%83%83%E3%83%81%E6%95%B0) を計算する関数を作ってみる。

```cpp
long fibonacci(int n) {
  if (n <= 0) {
    return 0;
  }
  if (n == 1) {
    return 1;
  }
  return fibonacci(n - 2) + fibonacci(n - 1);
}
```

フィボナッチ数は二つ前と一つ前のフィボナッチ数を足せばいいので、

`fibonacci(n - 2) + fibonacci(n - 1)` と書いて、関数の中で自分自身の関数を実行 (*再帰呼び出し*) している。

試しに、この関数に `6` の入力が入ったときの動作を追いかけてみよう。

> 1. `fibonacci(6)` は `fibonacci(4) + fibonacci(5)` を実行
>    1. `fibonacci(4)` は `fibonacci(2) + fibonacci(3)` を実行
>       1. `fibonacci(2)` は `fibonacci(0) + fibonacci(1)` を実行
>          1. `fibonacci(0)` は `1` を返す
>          2. `fibonacci(1)` は `1` を返す
>       2. `fibonacci(3)` は `fibonacci(1) + fibonacci(2)` を実行
>          1. `fibonacci(1)` は `1` を返す
>          2. `fibonacci(2)` は `fibonacci(0) + fibonacci(1)` を実行
>             1. `fibonacci(0)` は `1` を返す
>             2. `fibonacci(1)` は `1` を返す
>    2. `fibonacci(5)` は `fibonacci(3) + fibonacci(4)` を実行
>       1. `fibonacci(3)` は `fibonacci(1) + fibonacci(2)` を実行
>          1. `fibonacci(1)` は `1` を返す
>          2. `fibonacci(2)` は `fibonacci(0) + fibonacci(1)` を実行
>             1. `fibonacci(0)` は `1` を返す
>             2. `fibonacci(1)` は `1` を返す
>       2. `fibonacci(4)` は `fibonacci(2) + fibonacci(3)` を実行
>          1. `fibonacci(2)` は `fibonacci(0) + fibonacci(1)` を実行
>             1. `fibonacci(0)` は `1` を返す
>             2. `fibonacci(1)` は `1` を返す
>          2. `fibonacci(3)` は `fibonacci(1) + fibonacci(2)` を実行
>             1. `fibonacci(1)` は `1` を返す
>             2. `fibonacci(2)` は `fibonacci(0) + fibonacci(1)` を実行
>                1. `fibonacci(0)` は `1` を返す
>                2. `fibonacci(1)` は `1` を返す

↑ の `fibonacci(3)` や `fibonacci(4)` に注目。

こいつらはもう既に計算されているのに、何回も同じ処理がされている。

実際、上のプログラムは簡潔だけれどめちゃくちゃ計算が遅い。

`6` 番目を計算するだけでも `24` 回も関数呼び出ししているからね。

`n` 番目の計算には `黄金比の n 乗` の呼び出しが必要になってる。

> ※関数呼び出しは、足し算や変数の読み書きと比べるとちょっと時間がかかる。何十回もする場合はチリツモで遅い。

なので、↓ のようにループで書いたほうが速い。

```cpp
long fibonacci(int n) {
  if (n <= 0) {
    return 0;
  }
  long back_back = 0, back = 0, current = 1;
  for (auto i = 1; i < n; ++i) {
    back_back = back;
    back = current;
    current = back_back + back;
  }
  return current;
}
```

あと、数学的な知識があるなら数式で計算できる。

```cpp
#include <cmath>

long fibonacci(int n) {
  using std::round;
  using std::pow;
  using std::sqrt;
  // round (((1 + √5) / 2)^n - ((1 - √5) / 2)^n) / √5
  return round(
    (pow((1 + sqrt(5)) / 2, n) - pow((1 - sqrt(5)) / 2, n)) / sqrt(5)
  );
}
```

さすがに ↑ のコードは Wikipedia 見て書いたよ。数学者じゃないし。


## ランダウの記号

こういう計算にかかる時間を表す記号として、*O* を使った記法がある。

*O* のあとに続く括弧の中身でその計算の複雑さを示すものさしだよ。

*O*(1) みたいに書く。

*O* は **オーダー** と読む。

以下は入力を `n` としたときの記法だよ。

| 記法            | 名前         | 該当するアルゴリズム例                       |
| --------------- | ------------ | -------------------------------------------- |
| *O*(1)          | 定数時間     | 多項式の計算、整数の偶奇判定                 |
| *O*(log n)      | 対数時間     | 二分探索                                     |
| *O*(n)          | 線形時間     | コンテナの全探索 (一重ループ)                |
| *O*(n log n)    | 線形対数時間 | (優れた)ソートアルゴリズム、高速フーリエ変換 |
| *O*(n^a), 1 < a | 多項式時間   | データの探索 (a 重ループ)                    |
| *O*(2^n)        | 指数時間     |                                              |
| :               | :            | :                                            |

他にも呼び方はあるけれど、あんまり出てこないので省略。

それでは、書いたプログラムがどの *O* なのか見ていこう。


### 定数時間

`n` が偶数かどうか `bool` で返す関数。処理は一定。

```cpp
bool is_even(long n) {
  return n % 2 == 0;
}
```

---

`n` の符号を返す関数。分岐があっても処理は一定 (最大 2 回)。

```cpp
int sign(int n) {
  if (n < 0) {
    return -1;
  }
  if (0 < n) {
    return 1;
  }
  return 0;
}
```

こういうのは *O*(1) だよ。


### 線形時間

`1` から `n` までをすべて足す関数。

これは必ず `n` 回の足し算をする。

```cpp
int sum_from_1_to(int n) {
  int sum = 0:
  for (int i = 1; i < n; ++i) {
    sum += i;
  }
  return sum;
}
```

---

`data` の中から `target` があるかどうか探索する関数。

これは、最大で `data.size()` 回の比較をする。

データによっては `1` 回で終わることもあるけれど、

どんなデータが来るかはわからないので最悪の状況を考える。

```cpp
bool exists(std::vector<int> const &data, int target) {
  for (auto &e : data) {
    if (e == target) {
      return true;
    }
  }
  return false;
}
```

---

`0` から `end` 以下の偶数を順に出力する関数。

`end / 2` 回出力しているけれど、

`end` と処理回数が比例していることは変わらないので、これも *O*(n)。

```cpp
#include <iostream>

void print_even(int end) {
  for (int i = 0; i <= end; i += 2) {
    std::cout << i << "\n";
  }
}
```

だから、こいつらは *O*(n) にあたるよ。


### 平方 (二乗) 時間

`n` × `n` の掛け算表を出力する関数。

`9` を渡せば九九表になる。

`n^2` の計算と出力をしてる。

```cpp
#include <iostream>
#include <iomanip> // std::setw 関数が入っている

void print_mul_table(int n) {
  auto width = std::to_string(n * n).size();
  for (int y = 1; y <= n; ++y) {
    for (int x = 1; x <= n; ++x) {
      // std::setw は直後の入力を右寄せで幅を付ける
      // 実行してみれば分かるかも
      std::cout << std::setw(width) << x * y << ", ";
    }
    std::cout << "\n";
  }
}
```

---

`vertexes` は、`pair<int, int>` の座標を格納している。

この座標どうしの距離をすべて調べて、最も小さい距離を返す関数。

`vertexes.size()` を `n` とすると、`n (n - 1) / 2` 回の距離計算と比較をしている。

```cpp
double min_dist_between(std::vector<std::pair<int, int>> const &vertexes) {
  double min_dist = 1 / 0.0;
  for (auto it = vertexes.begin(); it < vertexes.end() - 1; ++it) {
    for (auto to_comp = it + 1; to_comp < vertexes.end(); ++to_comp) {
      using std::pow;
      using std::sqrt;
      double dist = sqrt(pow(it->first - to_comp->first, 2) + pow(it->second - to_comp->second, 2));
      if (dist < min_dist) {
        min_dist = dist;
      }
    }
  }
  return min_dist;
}
```

こういうのは *O*(n^2) だね。


## 主張なアルゴリズムの計算量

最近のコンピュータは、だいたい 1 秒間で 10 の 7~8 乗回の計算ができる。

競技プログラミングの問題には以下のように入力の制約が書かれているので、それで試算できる。

> 実行時間制限: 2 sec / メモリ制限: 1024MB
> 
> (中略)
>
>
> ## 問題文
> 英小文字からなる文字列 `S` が与えられます。以下の条件をみたす最大の正整数 `K` を求めてください。
> 
> * `S` の空でない `K` 個の文字列への分割 `S = S_1 S_2 ... S_K` で> あって `S_i ≠ S_i+1 (1 ≦ i ≦ K - 1)` を満たすものが存在する。
> 
> ただし、`S_1, S_2, ..., S_K` をこの順に連結して得られる文字列のこ> とを `S_1 S_2 ... S_K` によって表しています。
> 
> 
> ## 制約
> 
> * `1 ≦ |S| ≦ 2 × 10^5`
> * `S` は英小文字からなる
> 

[AtCoder Grand Contest 037 A - Dividing a String](https://atcoder.jp/contests/agc037/tasks/agc037_a) より引用

この問題の場合は、文字列 `S` の長さが `2e5` 以下となっている。

制限時間が 2 秒なので、`2e8` 回以上の計算は間に合わなくなりそう。

多分 *O*(|S|^2) のプログラムだと制限時間に間に合わない。

*O*(|S|) で解くように考えないといけない (実際解ける)。

こういう感じで考えるとプログラムを書く時間の無駄がないかも。

逆に余裕があるなら、ちょっと時間のかかるプログラムでも早く書けるなら書いたほうがいいっていう感じ。
