# 引数

いんすう派は賢いな

---

関数を作ったとはいえ、ずっと同じ処理をするだけじゃいろいろ不便。でしょ?

そこで、引数というものを作って、この呼び出すと同時に値を渡してもらうことができる。

これはさっきのページで言った、



にあたる関数ってわけ、

この引数リストは、関数の文法での

> void *名前* ( *引数リスト* )
>
> {
>
>   *0個以上の文*
>
> }

この丸括弧の中に `,` で区切って変数のように書く。

例えば、こんな感じになる。

```cpp
#include <iostream>

// 数を出力する
void print_number(int number) {
  std::cout << "Number: " << number << "\n";
}

// 2 つの数の合計が正の数ならそれを出力する
void print_sum_if_positive(int alpha, int beta) {
  int sum = alpha + beta;
  if (0 < sum) {
    print_number(sum);
  }
}

int main() {
  // 数値を変えて遊んでみよう
  print_number(63);
  print_number(-43);
  print_sum_if_positive(63, -43);
}
```
