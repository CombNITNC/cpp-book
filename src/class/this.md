# this

ほら、コレだよコレ

---

メンバ関数では、`this` という特別な値が使える。

こいつはメンバ関数を呼び出すのに使ったオブジェクトのポインタだ。

```cpp
struct Hoge {
  void foo() {
    this;
  }
};
Hoge hoge;
hoge.foo(); // この場合の this は &hoge と同じ

Hoge{}.foo(); // この場合の this が指す実体は、式が終わるとなくなる
```

他のメンバへのアクセスは `this` を介しているけれど、省略できちゃう。

```cpp
class Position {
  int _x, _y;
public:
   int x() const {
     return this->_x; // 本当はこうする
   }
   int y() const {
     return _y; // this-> は省略できるってだけ
   }
};
```

`this` がどこから来ているのかイメージしにくかったら、こういうプログラムなんだと思ってみてね。

```cpp
struct Hoge {};
void foo(Hoge +this) { /* ... */ }

Hoge hoge;
foo(&hoge);
```
