# メンバ関数呼び出し

関数が生えてる！？

---

いよいよインスタンスの本領発揮タイムだよ。

インスタンスは、`.` に続けて、そのクラスのその **インスタンスについてる関数を呼び出せる** よ。

この関数のことを、そのクラスの *メンバ関数* っていう。

試しに `std::string` クラスだと、こんなメンバ関数を呼び出せる。

```cpp
#include <iostream>
#include <string>

int main() {
  std::string text = "Hello";
  std::cout << text << "\n"; // Hello


  // append、末尾に追加
  text.append(", World");
  std::cout << text << "\n"; // Hello, World

  text += ", World"; // 上と同じ (このクラスの機能)
  std::cout << text << "\n"; // Hello, World, World

  text.append(2, '！'); // 2 個の '！'
  std::cout << text << "\n"; // Hello, World, World!!


  // insert、指定位置に挿入
  // 注意！ 位置は 0 から数える
  text.insert(0, ">> "); // 0 番目に ">> "
  std::cout << text << "\n"; // >> Hello, World, World!!

  text.insert(22, 3, '.'); // 22 番目に 3 個の '.'
  std::cout << text << "\n"; // >> Hello, World, World...!!


  // replace、指定範囲を書き換え
  text.replace(0, 1, ""); // 0 番目から 1 文字を "" に
  std::cout << text << "\n"; // > Hello, World, World...!!
  
  text.replace(14, 10, 2, '~'); // 14 番目から 10 文字を 2 個の '~' に
  std::cout << text << "\n"; // > Hello, World~~!!


  std::string sub; // 保管用にもう一個
  // substr、指定範囲をコピー
  sub = text.substr(2); // 2 番目から最後まで
  std::cout << sub << "\n"; // Hello, World~~!!

  sub = sub.substr(7, 5); // 7 番目から 5 文字まで
  std::cout << sub << "\n"; // World
}
```

こいつらを使いこなすだけでも、基本的なテキスト処理ができちゃう。

この `std::string` は別に暗記とかしなくてもいいよ。ネットで調べればいくらでも出てくる。

**クラスのインスタンス** から **メンバ関数を呼び出す** 流れだけは忘れないでね。
