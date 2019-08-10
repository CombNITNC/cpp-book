# ラムダ式

|出口|　λ............ﾄﾎﾞﾄﾎﾞ

---

## `auto`

`auto` はこれから学ぶのに必要な知識なので、一緒に解説しておく。

`auto` は、少し特殊な型だよ。変数ではその初期値から、その型を推論するんだ。

```cpp
auto number = 0; // int 型
auto bigger = 0L; // long 型
auto wonderful = 0LL; // long long 型

auto single = 1.2f; // float 型
auto doppio = 2.4; // double 型

auto text = std::string("Hey!"); // std::string 型
```

戻り値型では、戻り値の型から推論される。引数には使えない (そのうち使えるようになるかも)。

```cpp
auto add(int a, int b) { // int add(int, int) になる
  return a + b; // int + int は int だからね
}

auto sub(int a, int b) -> int { // auto と書いても、-> の後に明示できる
  return a - b;
}
```

長ったらしいイテレータクラスには、めちゃくちゃ有効。

```cpp
std::vector<int> data;

std::vector<int>::iterator head = data.begin(); // 長い
auto tail = data.end(); // 短い
```


## ラムダ式

いままでの関数は `main` 関数の外にぽつぽつと書いてた。

ラムダ式を使うと、`main` 関数といった **関数の中ででも関数が作れる**。

文法はこう。

> [] { *0 個以上の文* }

```cpp
#include <iostream>

int main() {
  [] {
    std::cout << "Hello!\n";
  };
}
```

### 呼び出し

これは関数を作っただけで特に実行していない。

普通の関数と同じように、後ろに `()` をつけて呼び出せば実行できる。

```cpp
#include <iostream>

int main() {
  [] {
    std::cout << "Hello!\n";
  }();
}
```


### 変数への代入

ラムダ式はれっきとした **式** であり、変数に代入できちゃう。特定の型はないけれど、`auto` 型変数は使える。

```cpp
auto func = [] {
  std::cout << "wow wow! ";
};
func();
func();
```


### 引数を受け取る

ラムダ式で引数を受け取るときは、`[]` と `{}` の間に、普通の関数と同じように引数リストを書く。

> [] ( *引数リスト* ) { *0 個以上の文* }

```cpp
auto show = [] (int x) {
  std::cout << "X is " << x << "\n";
};
show(2);
show(40);
```

引数がないときは、引数リストの `()` は省略できるってだけ。その方が見やすいのかな?


### 値を戻す

ラムダ式で戻り値を作るときでも、特に型の指定は要らない。

```cpp
auto add = [] (int a, int b) { return a + b; };
add(2, 4);
add(3, add(5, 8));
```

引数リストの `)` の後ろに `-> 型` って書いて明示することもできる。

```cpp
auto sub = [] (int a, int b) -> int { return a - b; }
sub(4, 2);
sub(sub(8, 5), 1);
```


### ラムダキャプチャ

ラムダ式が書いてあるスコープと同じところの変数にアクセスする場合は、

**ラムダキャプチャ** を書くことで、使用できるようになる。

ラムダキャプチャを書くのは、`[]` の中だよ。そのためにあるからね。


#### コピーキャプチャ

`[]` の中に変数名をそのまま書くと、

その変数の数値をコピーして *ラムダ式で作った関数が値を保持する*。

```cpp
auto adder(int a) { // 関数を返す関数
  return [a] (int b) {
    return a + b;
  };
  // 関数の処理が終わると引数の a はもう使えなくなるはず
}

int main() {
  auto add3 = adder(3);
  // add3 は、a の値をコピーした 3 と引数を足して返す
  add3(2); // 5
  add3(4); // 7
  auto add10 = adder(10);
  add10(4); // 14
}
```


#### 参照キャプチャ

コピーキャプチャの文法で、変数名の前に `&` をつけると参照キャプチャになる。

コピーキャプチャとは違って変数の値を変えられる。

```cpp
int count = 0;
auto counter = [&count] { ++count; };
auto show = [count] { std::cout << "Count: " << count << "\n"; }
counter();
counter();
counter();
show();
```

ただし、使えなくなっているはずの変数にもアクセスできてしまう。挙動は不定になる。

```cpp
#include <iostream>

auto lazy_presenter(int a) { // 関数を返す関数
  return [&a] () {
    std::cout << a << "\n"; // おかしな動作をするかもしれない
  };
  // 関数の処理が終わると引数の a はもう使えなくなるはず
}

int main() {
  auto presenter = lazy_presenter(2);
  presenter();
}
```

ラムダキャプチャは他にも機能があるけれど、それはまたの機会に。
