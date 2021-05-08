# 文字列

ちなみに string は日本語で糸なんだけど、いまいちイメージに合わないので文字列と訳す

---

何気なく使っていた `"これ"` だけど、もう少し解説しておく。

この文字列 (C++ では C文字列 ともいわれる) は、連続した `char` の塊。

**ヌル文字** という文字 (`'\0'`) があならず最後に来る。

```cpp
char text[] = { 'B', 'E', 'A', '\0' };
```

↑ と ↓ は同じ！

```cpp
char text[] = "BEA"; // こう見えて 4 要素ある
```

配列と違ってヌル文字が必ずあるので、長さに依存しない、こんなループ処理が書ける。

```cpp
#include <iostream>

int main() {
  char text[] = "BEA";
  for (int i = 0; text[i] ！= '\0'; ++i) {
    ++text[i]; // 文字が一つ次に進む
  }
  std::cout << text; // CFB
}
```

ただ、文字列だと文字を足したりくっつけたりとかはできない。

同じことは `std::string` でもできる。

```cpp
#include <iostream>
#include <string>

int main() {
  std::string text = "BEA";
  for (int i = 0; i < text.size(); ++i) {
    ++text.at(i); // at メンバ関数で要素にアクセス
  }
  std::cout << text; // CFB
}
```

あんまり文字列を使うメリットはないので、変更したりするときは絶対に `std::string` を使おうね。
