# クラスのメンバ

ﾅｶｰﾏ

---

クラスにメンバを定義すると、インスタンスからメンバにアクセスできる。


# メンバ変数

メンバ変数を定義すると、クラスのオブジェクトがそれを所有するようになる。

```cpp
class Position {
  int x, y;
  double norm;
  :
  :
};
Position p; // p の中には x と y がある
```


# メンバ関数

メンバ関数を定義すると、クラスのオブジェクトに関数がくっつく (厳密には所有じゃない)。

```cpp
class Position {
  void add(Position const&);
  double abs() const; // ← の const 修飾をつけると、
  :
  :
};
Position p; // p
```


# アクセス指定子 `public` / `private`



## `public`



## `private`


