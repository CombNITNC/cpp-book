# if

もしも〜♪(ｳﾞｪｯ)君が〜♪(ｳﾞｯ)一人なら〜♪(ｳﾞｪｯｳﾞｪｯ)

---

それでは `if` 文を学ぼう。

これは特殊な文で、`bool` 型の値を使って **分岐する処理** が書ける。

`bool` が C++ で二番目に活躍するぞ。


文法はこう。

後に文を続けて書くけど、これはもっぱら複文にする。

> if ( *真偽値の式* ) *式が true のときに実行する文*

比較演算子と組み合わせれば、**条件を満たす時にする処理** が書ける。

```cpp
int value;

std::cin >> value;

if (0 < value) {
  // value が 0 より大きい時だけ実行される
  std::cout << "0 よりおっきい";
}

if (value < 0) {
  // value が 0 より小さい時だけ実行される
  std::cout << "0 よりちっちゃい";
}

// value が 0 ぴったりのときは何も起きない
```

条件を満たさないときは、そのまま文が飛ばされて下に流れる。


`if` の波括弧の **始まりを書いたら、すぐに終わりも書く** ように心がけよう。

更に、基礎でも書いたように **インデントして** 波括弧が **どのくらい入れ子になっているか見やすいように** すべし。

他人や未来の自分がコードを見るときに非常に見づらいから。


もちろん `if` の中にも `if` を書ける。

```cpp
int value;

std::cin >> value;

if (0 <= value) {
  if (value <= 10) {
    // value が 0 以上で 10 以下のときだけ実行される
  }

  // value が 0 以上であれば実行される

  if (value < 0) {
    // ここが実行されることはありえない
  }
}
```

いろいろ組み合わせて試してみようぜ。
