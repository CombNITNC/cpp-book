# 章末問題



---

以下の問題を用意しておきました。解いてプログラミングになれましょ。


## 例題 1

部分和数え上げ問題だぁよ。

> 入力:
> ```
> N
> S
> A1 A2 A3 ... AN
> ```
> * `N` は 入力 `A1, A2, A3, ...` の総数を表す整数
> * `S` はただの整数
> * `A1, A2, A3, ...` は正の整数
> 
> 問題:
>
> `A1, A2, A3, ...` の中からいくつかを組み合わせて合計して、ぴったり `S` にすることができる場合の数を求める。
>
> `S` にできる場合の数を出力せよ。

入力:
```
5
12
7 5 3 1 8
```

出力:
```
2
```

### 解き方

DP (動的計画法) を使うと楽だよ。

今回もかんたんなケースから考えよう。

> `0` ~ `0` 番目までの中から組み合わせた合計に `0` がある場合の数

この場合の数は `1` 通りだよ。

```cpp
1;
```

片方の数字を変数にして、グレードアップしよう。

> `0` ~ `index` 番目までの中から組み合わせた合計に `0` がある場合の数

この答えを保存する `vector<long> dp` というコンテナを用意する。

※ `long` にしてあるのは、組み合わせの数は大きくなる (*組み合わせ爆発* する) ことが多いから。それでも足りないなら、大きい数で割った余りにする (この場合は問題文に書いてある) か、`long long` やいくらでも入る整数 (多倍長整数) 型を使うことになる。

`dp[i]` には、`0` ~ `i` 番目までにおける場合の数を格納するってこと。

`dp[index]` に `dp[index - 1]` を代入してから、

* `A[index - 1]` が `0` の場合
  * `dp[index]` を `1` 増やす。

この `index` を `for` で `1` から `N` まで繰り返せば、`dp.back()` に答えが入る。

```cpp
vector<long> dp(N + 1); // dp[N] までなので 1 個余分に必要

// さっき求めた かんたん なやつ
dp[0] = 1;

for (size_t index = 1; index <= N; ++index) {
  dp[index] = dp[index - 1];
  if (A[index - 1] == 0) {
    dp[index] += 1;
  }
}

dp.back(); // これが答え
```

OK、もう片方の数字も変数にしよう。

> `0` ~ `index` 番目までの中から組み合わせた合計に `sum` がある場合の数

さっきの `dp` を `vector<vector<long>>` に改良する。

`dp[i][j]` には、`0` ~ `i` 番目までにおいて総和を `j` とした問題の答えを格納する。

これも `dp[index][sum]` に `dp[index - 1][sum]` を代入してから、

* `A[index - 1] <= sum` の場合 (↓ の添字が 0 以上になるように範囲チェック)
  * `dp[index][sum]` を `dp[index - 1][sum - A[index - 1]` の数だけ増やす。

この `index` を `1` から `N` まで、 `sum` を `0` から `S` まで繰り返せば、`dp.back().back()` に答えが入る。

```cpp
vector<vector<long>> dp(N + 1, vector<long>(S + 1));

dp[0][0] = 1;

for (size_t index = 1; index <= N; ++index) {
  for (size_t sum = 0; sum <= S; ++sum) {
    dp[index][sum] = dp[index - 1][sum];
    if (A[index - 1] <= sum) {
      dp[index][sum] += dp[index - 1][sum - A[index - 1]];
    }
  }
}

dp.back().back(); // 答えがでたぁ
```

最終的に、こうなる。

```cpp
#include <iostream>
#include <vector>

int main() {
  using std::cin;
  using std::vector;

  int N, S;
  cin >> N >> S;

  vector<int> A(N);
  for (auto &elem : A) {
    cin >> elem;
  }

  vector<vector<long>> dp(N + 1, vector<long>(S + 1));

  dp[0][0] = 1;

  for (size_t index = 1; index <= N; ++index) {
    for (size_t sum = 0; sum <= S; ++sum) {
      dp[index][sum] = dp[index - 1][sum];
      if (A[index - 1] <= sum) {
        dp[index][sum] += dp[index - 1][sum - A[index - 1]];
        /* DP テーブル 観賞用
        for (std::vector<long> const &column : dp) {
          for (long elem : column) {
            std::cout << elem << "|";
          }
          std::cout << "\n";
        }
        std::cout << "\n\n";
        */
      }
    }
  }

  using std::cout;

  cout << dp.back().back() << "\n";
}
```


## 問 1

*ナップサック問題* だよ。

> 入力:
> ```
> N
> C
> V1 V2 V3 ... VN
> W1 W2 W3 ... WN
> ```
> * N は 入力 V1, V2, V3, ... と W1, W2, W3, ... の総数を表す整数
> * C は正の整数
> * VN と WN は正の整数
> 
> 問題:
>
> ナップサックに、効率よくモノを詰め込みたいんだけど、
> 
> このナップサックでは `C` 以上の重さになると運べない。
>
> `V[i]` は `i` 番目のモノの 価値 を表している。
>
> `W[i]` は `i` 番目のモノの 重さ を表している。
>
> 重さの合計が `C` を超えないように & 価値が最大になるようにモノを選んだときの、価値の合計を求める。
>
> この 価値の合計 を出力せよ。

入力:
```
6
8
3 2 6 1 3 85
2 1 3 2 1 5
```

出力:
```
91
```

ヒント:

動的計画法と DP テーブルを使うのなら、

`dp[index - 1][weight - W[index - 1]] + V[index - 1]` が、`index` 番目以前のモノを詰め込む問題での答え。


## 問 2

共通部分列というのがありまして、

例えば `"AGCAT"` と `"GAC"` の共通部分列は、

```
*A* G *C* A T
G *A* *C*


A *G* *C* A T
*G* A *C*

A *G* C *A* T
*G* *A* C
```

で、`"AC", "GC", "GA"` というふうな感じ。

※ちなみにこの A G C T は DNA とかの塩基配列のやつ。遺伝子のパターンを調べるのにも役立つ問題ってこと。

この共通部分列の中で最も長いやつを、最長共通部分列っていう。まんまだね。

それじゃ、この最長共通部分列 (LCS、Longest Common Subsequence) をプログラムで求めましょ。

> 入力:
> ```
> S
> T
> ```
> * `S` と `T` は文字列
> 
> 問題:
>
> `S` と `T` の 最長共通部分列の長さ を出力せよ。

入力:
```
abcde
acbef
```

出力:
```
3
```
(LCS は "ace", "abe")

ヒント:

DP テーブルには、`S` の `0` ~ `i` 文字目までと `T` の `0` ~ `j` 文字目までの LCSの長さ を `dp[i][j]` で格納するといいかも。

こうしておくと、

1. `dp[i][j] = std::max(dp[i - 1][j], dp[i][j - 1])` 
2. `S[i - 1] == T[j - 1]` のときに、`dp[i][j] = std::max(dp[i][j], dp[i - 1][j - 1] + 1)` 

ってすれば求まるからね。
