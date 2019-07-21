# 参照型

[こちら](./ref.md) を参照してください

---

`const` に続いて、こっちも型を拡張する機能だよ。

変数名の前にこのように `&` をつけ、同じ型の変数で初期化すると、

```cpp
int body = 0;
int &ref = body;
```

変数に対して、それを *参照する型* の変数を作ることができる。

言い換えると、変数の純粋な別名になって、変数を間接的に変更できる。

```cpp
int body = 0;
int &ref = body;

ref = 3;
body; // これも 3 になる

body = 2;
ref; // これも 2 になる
```

`&` は変数名の前につける。

だから、同時に宣言する場合はこうなる。

```cpp
int body = 0, &ref = body;
// body は int 型で、ref は int& 型になる
```

## 参照の伝播

参照型変数に入っている参照は、さらに別の参照型変数に渡せる。

つまり、参照は又貸しできちゃう。

```cpp
int body = 1;
int &ref = body;
int &ref_ref = ref;
```


## 参照の引数

引数にした場合、関数の外にある変数を直接変更できる。

```cpp
int write_2(int &target) {
  target = 2;
}

int main() {
  int number = 0;
  write_2(number);
  numer; // 2
}
```

ちなみに、`std::cin` もこれを利用して変数に書き込んでいる。

```cpp
#include <iostream>

int main() {
  int input;
  std::cin >> input; // input の参照を渡している
}
```
