# const の参照

( →_→)ｱｸｾｽ！

---

今度は `const` と参照を組み合わせてみよう。

```cpp
int value = 0;
int const &ref = value;

// いろいろしてみる
int read = ref; // OK、読み込める
ref = 2; // NG、書き込めない
```

このように、**`const` な変数への参照** という扱いになる。

*他の変数から値を読み取るだけ* の場合に、コピーを防ぐときに使う感じ。

```cpp
#include <string>

std::string add_quote(std::string const& message) {
  return "「" + message + "」"; // ここでは複製される
}

std::string add_quote_copy(std::string message) { // ここで複製されて
  return "「" + message + "」"; // 更にここで複製される
}

#include <iostream>

int main() {
  std::string text = "abc";
  {
    std::string quote_text = add_quote(text); // text は複製されない
    std::cout << quote_text << "\n";
  }
  {
    std::string quote_text = add_quote_copy(text); // text は複製される
    std::cout << quote_text << "\n";
  }
}
```

この例は一回しかコピーが起こらないので大した違いはないけど、

大量のデータを扱い始めると問題が顕現してくる。


ただし、`int` みたいな小さいサイズのを `const&` で渡してもあんまり美味しくない。

- 参照型のサイズ (大抵 `unsigned long` と同じ) より大きいサイズのもの
- 複製する必要がないのにコピーすると遅いもの

とかにはよく効く。
