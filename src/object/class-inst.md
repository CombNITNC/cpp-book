# クラスとインスタンス

クラスの世界へようこそ

---

ここから、**クラス** をどんどんやっていく。当たり前のように使うことになるから覚えよう。

あ、教室のことじゃないよ。

言語に最初からあるものじゃなくて、ちょっと特殊な *プログラム内で作った型*。しかも既にたくさん用意されている。

今回は `string` ファイルの `std::string` クラスを使ってみよう。

これはテキストを変数に格納して操作できるクラスだよ。

他にもいろんなクラスはあるけど、まずこれで慣れよう。

```cpp
#include <iostream>
#include <string>

int main() {
  std::string s;

  std::cout << s; // まだ何も出ない
}
```

うん。変数はいつもどおり。

この変数 `s` には、ここで初登場の特殊なものが入る。

この入ったものをクラスに対して **インスタンス** という。

インスタンスを変数に入れずに直接作ることもできる。

その場合は関数呼び出しのように、クラスを使う。

```cpp
std::string(); // インスタンスが作られるが変数には入らない
std::string text = std::string(); // 変数に初期化
std::string label; // ↑はなくても同じ

// これは関数の宣言
std::string question(); // 動くけど変数ではない
```

クラスは、**インスタンスを作る関数またはその型** だと思って大丈夫。

インスタンスを作るのにも初期化ができる。

`std::string` には *文字列* を渡して初期化できる。

初期化には 3 種類の構文がある。どれを使うかはお好みで。

```cpp
// 関数風
std::string morning = std::string("Good morning！\n");

// 渡す初期値が一つだけのとき使える
std::string afternoon = "Good afternoon.\n";

// 変数風
std::string evening("Good evening...\n");

std::cout << morning << afternoon << evening;
```

ちなみに、この文法は今までの型にもちゃんと使える。

```cpp
int a = int(2);

int b = 3;

int c(4);

std::cout << a << b << c;
```
