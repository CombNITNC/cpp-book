# bool

正しいか正しくないか

---

新しい型 `bool` の登場だ。*真偽値* という、ある物事が **正しい** か **正しくない** かを表す値を扱う。

`true` は何かが正しいことを表す。

`false` は何かが正しくないことを表す。

```cpp
bool zero_is_0;
zero_is_0 = true;
bool you_are_me;
you_are_me = false;
```


## 整数に型変換する場合

* `false` => `0`
* `true` => `1`

整数と四則演算させようとすると、こんな感じの不思議な挙動になっちゃう。

```cpp
3 * false; // => 0
4 + true; // => 5
```


## 他の型から `bool` に型変換する場合

* `0` (もしくは `0` と同じ何か) は `false`
* それ以外はすべて `true`

これはかなり直感的ではないので、使わないようにするよ。


## std::cout で出力するとき

`std::cout` で普通に出力しようとすると、勝手に `0` と `1` に変換されちゃう。わけがわからないよ。

```cpp
#include <iostream>
int main() {
  std::cout << true;
}
```

`true` や `false` のまま出力するには、`std::boolalpha` (これは関数) をそれより前に流し込む必要がある。

```cpp
#include <iostream>
int main() {
  std::cout << std::boolalpha << true;
}
```

これを取り消して元に戻す場合は、`std::noboolalpha` を流す。

```cpp
#include <iostream>
int main() {
  std::cout << std::boolalpha << true
    << std::noboolalpha << true;
}
```

めんどくさいけど、忘れないでおいてね。
