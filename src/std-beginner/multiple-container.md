# 多重コンテナ

コンテナを開けると、そこはコンテナだった

---

ここでは、`vector` の中に `vector` を入れるのをやっていくよ。

```cpp
std::vector<std::vector<int>>
```

このクラスを初期化するときは、こうなるよ。

```cpp
std::vector<std::vector<int>> matrix(2, // 2 個の ↓
  std::vector<int>(2, 1) // 2 個の 1
);
```

アクセスするときも、`at` が 2 個出てくる。

```cpp
std::vector<std::vector<int>> matrix(2,
  std::vector<int>(2, 1)
);

matrix.at(0).at(0); // 1
matrix.at(1).back(); // 1
matrix.back().back(); // 1
```

こういうのを *多重*、もしくは **多次元** なコンテナっていう。


## 使い所

多次元のコンテナは、小難しい問題を速く解くプログラムを作るときに重宝する。

ここではその例題をいくつか示してくよ。


### 動的計画法

**動的計画法** (DP、Dyanic Programming) は、いくつかの組み合わせに関する問題で役に立つ解法だよ。

まずは、*部分和問題* からいってみよう。

こんな感じの問題だよ。

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
> `A1, A2, A3, ...` の中からいくつかを組み合わせて合計して、ぴったり `S` にできるか求める。
>
> `S` にできる場合は `"YES"`、できない場合は `"NO"` に、改行を加えて出力せよ。

まずは *入力* を受け取るとこを書く。

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

  // ここからは、ココに書き足すコードだけを示すよ
}
```

こういうパッと見て解き方がさっぱり分からん問題は、**かんたんなケースを考えてから難しくしていく** のがいい。

**一段階むずかしい問題の答え** に、**よりかんたんな問題の答えを使って答える** 考え方が大事だよ。

この問題の一番 *かんたん* なケースを考えよう。

> 入力を `0` 個組み合わせて `0` が作れるかどうか

これが一番 *かんたん* なんだけど、これは必ず `true` だよ。

```cpp
true;
```

虚無の合計だけど、とりあえず `0` だと考える。いいね?

片方の数字を変数にして、グレードアップしよう。

> 入力を `n` 個組み合わせで `0` が作れるかどうか

この答えは、*かんたん* な問題の答えを使って求められる。

* `n - 1` 個の組み合わせで `0` が作れるかどうか

を満たせば、`n` 個の組み合わせでも必ず `0` が作れるよ。

この答えを保存する `vector<bool> dp` というコンテナを用意する。

`dp[i]` には、「`i` 個組み合わせて `0` が作れるかどうか」を格納することにする。

変数 `n` を `for` で `1` から `N` まで繰り返せば、

1. `dp[n - 1]` が `true` になっている

ときに `dp[n]` に `true` を代入すると、`dp.back()` に答えが入る。

```cpp
vector<bool> dp(N + 1); // dp[N] までなので 1 個余分に必要

// さっき求めた かんたん なやつ
dp[0] = true;

for (size_t n = 1; n <= N; ++n) {
  bool prev = dp[n - 1]; // prev は previous の略で、前のやつ って意味
  dp[n] = prev;
}

dp.back(); // これが答え。今回は必ず true だけど
```

OK、もう片方の数字も変数にしよう。

> 入力を `n` 個組み合わせて `sum` が作れるかどうか

この答えは、*かんたん* な問題の答えを使って求められる。

* `n - 1` 個の組み合わせで `sum` が作れるかどうか
* `n - 1` 個の組み合わせで `sum - A[n - 1]` が作れるかどうか

のどっちかを満たせば、`n` 個の組み合わせで `sum` が作れるよ。

二つ目の条件は、`n` 個目の入力を選んだうえで問題の条件を満たすときを意味している。

さっきの `dp` を `vector<vector<bool>>` 型に改良する。

`dp[i][j]` には、「`i` 個の組み合わせで `j` が作れるかどうか」を格納する。

変数 `n` を `1` から `N` まで、 `sum` を `0` から `S` まで繰り返せば、

1. `dp[n - 1][sum]` が `true` になっている
2. `A[n - 1] <= sum` の場合 (↓ の添字が 0 以上になるように範囲チェック)
   1. `dp[n - 1][sum]` もしくは `dp[n - 1][sum - A[n - 1]` が `true` になっている

ときに、`dp[n][sum]` に `true` を代入すると、`dp.back().back()` に答えが入るんだぜ。

```cpp
vector<vector<bool>> dp(N + 1, vector<bool>(S + 1));

dp[0][0] = true; // かんたん なやつ

for (size_t n = 1; n <= N; ++n) {
  for (size_t sum = 0; sum <= S; ++sum) {
    bool prev = dp[n - 1][sum];
    dp[n][sum] = prev;
    if (A[n- 1] <= sum) {
      dp[n][sum] = prev || dp[n - 1][sum - A[n - 1]];
    }
  }
}

dp.back().back(); // 答えじゃ!
```

最後の仕上げに、出力を作ってっと。

```cpp
using std::cout;

if (dp.back().back()) {
  cout << "YES\n";
} else {
  cout << "NO\n";
}
```

これで完成。

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

  vector<vector<bool>> dp(N + 1, vector<bool>(S + 1));

  dp[0][0] = true;

  for (size_t n = 1; n <= N; ++n) {
    for (size_t sum = 0; sum <= S; ++sum) {
      bool prev = dp[n - 1][sum];
      dp[n][sum] = prev;
      if (A[n- 1] <= sum) {
        dp[n][sum] = prev || dp[n - 1][sum - A[n - 1]];
      }
    }
  }

  using std::cout;

  if (dp.back().back()) {
    cout << "YES\n";
  } else {
    cout << "NO\n";
  }
}
```

こんなふうに、動的計画法では **DP テーブル** と言われる多次元のコンテナを作って解くことが多いよ。

DP テーブルの挙動がわかりにくかったら、中身を見たい行で、こうやって出力してみるといいかも。

```cpp
for (std::vector<bool> const &column : dp) {
  for (bool elem : column) {
    std::cout << elem << "|";
  }
  std::cout << "\n";
}
std::cout << "\n\n";
```

章末問題では、この応用となる *ナップサック問題* を出すから、解いてみてね。

