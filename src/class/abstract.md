# 抽象クラス

ふわふわ〜

---

# virtual

メンバ関数には、`virtual` という指定ができる。こんな関数を仮想関数っていう。

```cpp
class A {
  virtual void a() {}
};
```


## オーバーライド

クラスを派生するときに、派生元クラスと同じ形のメンバ関数を定義して上書きできる。

試しに、普通の関数を上書きしたクラスを用意して、そのオブジェクトを親クラスの型にぶち込んでみる。

```cpp
struct A { void a() { std::cout << "A\n"; } };
struct B : public A { void a() { std::cout << "B\n"; } };

B b;
A &ref = b;
ref.a(); // A
```

うん。オブジェクトは `struct B` のものなのに、`struct A` のメンバ関数を呼び出している。

これは、型が `struct A` になってるからメンバもそっちなんだと解決される。

この定義の上書きを **オーバーライド** って言う。

仮想関数でも同じことをやってみよう。

```cpp
// virtual の指定は派生元だけで OK
struct A { virtual void a() { std::cout << "A\n"; } };
struct B : public A { void a() { std::cout << "B\n"; } };

B b;
A &ref = b;
ref.a(); // B
```

あれ、`struct B` のメンバ関数が呼ばれた。

つまり、仮想関数にすると、実際のオブジェクトのメンバ関数が呼ばれるわけだ。

ちなみに、`override` を付けるとオーバーライドできていないときにコンパイラがエラーを出してくれる。

```cpp
struct A {
  virtual void a() { std::cout << "A\n"; }
};
struct B : public A {
  void a() override { std::cout << "B\n"; }
};
```


## 純粋仮想関数

仮想関数を定義するときに、定義の部分を `= 0` ってすると **純粋仮想関数** になる。

```cpp
class A {
  virtual void a() = 0;
};
```

純粋仮想関数が含まれるクラスは **抽象クラス** として扱われるよ。

抽象クラスは、オブジェクトを作ることができなくなるよ。

```cpp
A a; // エラー
```

でも、参照やポインタを扱うことはできる。

```cpp
void invoke(A &ref) {
  ref.a();
}
```

さっきみたいに、仮想関数をオーバーライドして使うのが前提になってるんだ。
