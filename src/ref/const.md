# const

そのままの君でいて

---

今までの *変数* は、スコープ内であれば文字通りどこでも値を書き換えることができた。

できたんだ。

これはたしかに便利だけど、*数の別名* として変数を使っていた場合は *変更できるとめんどくさいこと* になる。

```cpp
double tax = 1.08; // 税率 8% なので 1.08倍

int price = 500;
price *= tax;

// :
// :

tax = 1.10; // いつのまにか 10% に

// :
// :

price = 800;
price *= tax; // あれ?
```

変更できることが、逆に混乱を招いたりバグを生むことがある。


そこで型に `const` (定まった、constant の略) 修飾子をつけると、`const` な型になる。変数の型のところに使えば `const` な変数が作れる。

```cpp
// 前でも後でもいい
int const a;
const int b;
int const c, d; // 同時に 2 つ
```

この変数を作るときに値を入れることを *初期化* というのだけれど、この初期化のタイミングでしか値をセットできない。

そして、変数を作った後は **代入できなくなる**。コンパイラがどこかで **うっかり代入していないかチェックしてくれる** んだ!

```cpp
double const tax = 1.08; // 税率 8% = 1.08倍

int price = 500;
price *= tax;

// :
// :

tax = 1.10; // コンパイルできない

// :
// :

price = 800;
price *= tax; // 安心して tax が使える
```

`const` の値を別の変数にコピーすることはできる。

```cpp
const int magical = 4444;
int temp = magical;
```

引数にも `const` 指定はできる。

```cpp
int add(int left, int right) {
  left += right - right; // (意味はない)
  return left + right;
}

int add_const(int const left, int const right) {
  left += right - right; // NG
  return left + right;
}
```

現時点では役に立たないけど、次の次のページで応用的に使う。
