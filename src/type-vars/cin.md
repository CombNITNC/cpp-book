# std::cin

今度は入力

---

いよいよ **入力** を扱う時が来た。今までコンパイルする前に値を埋め込んでいたけど、プログラムを実行するタイミングで値を決められるようになるってこと。

ここで使うのは `std::cin` というもので、`std::cout` と同じ `iostream` ファイルに入っている。

この `std::cin` は `>>` 演算子 (正確には `std::cin` の `operator>>`  関数) を使って、いろんなデータを **入力** できるよ。`std::cin` から *取り出す* ようなイメージ。

```cpp
#include <iostream>
int main() {
  int input_number;
  std::cin >> input_number;
  std::cout << input_number;
}
```

上の例を実行して、黒い画面で **数字を入力 → Enter** してみると、何が出力されるでしょうか?

Wandbox の場合は、実行前に下の Stdin (標準入力 - Standard input の略) のテキスト欄に入れておくと、入力に使用されるようになってる。

この `std::cin` 最大の特徴が、いろんな型でそのまま入力を受け取れるところだよ。

```cpp
#include <iostream>
int main() {
  std::cout << "円の半径を入力してください。その面積を出力します。\n";
  double radius;
  std::cin >> radius;
  std::cout << radius * radius * 3.1416;
}
```

**複数の値を同時に受け取る** こともできるよ。これは左の変数から順に入力された値が入れられるよ。

```cpp
#include <iostream>
int main() {
  std::cout << "2 つの整数を入力してください。最初の値 - 次の値 を出力します。\n";
  int first, second;
  std::cin >> first >> second;
  std::cout << first - second;
}
```

こんな感じで **計算結果を返すプログラム** を自分で **アレンジして書いてみよう**。`std::cin >> ~` だけだとコンソール画面で何を入力すればいいのかわからない。先に `std::cout` で **何を入力すればいいか表示** したほうがいいかも。
