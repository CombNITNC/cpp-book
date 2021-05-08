# 引数

いんすう派は賢いな

---

関数を作ったとはいえ、*ずっと同じ処理をするだけの関数* じゃいろいろ不便。でしょ？

そこで、引数 (ひきすう) というものを作って、この呼び出すと同時に値を渡してもらえる。

この引数リストは、関数の文法で、

> void *名前* ( *引数リスト* )
>
> {
>
>   *0個以上の文*
>
> }

この **丸括弧の中** に `,` で区切って変数のように書く。

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
  print_sum_if_positive(63, -43); // 複数の値は , で区切って渡す
}
```

関数が受け取る予定の、名前を付けた引数は **仮引数** という。

対して、関数に渡す実際の値は **実引数** という。

まぁ、あんまり使わない用語だけれどね。
