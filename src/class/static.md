# 静的メンバ

えっちではない

---

今までのメンバはクラスのオブジェクトに関するものだった。

**静的メンバ** という、クラスの名前に所属しているメンバも定義できる。

静的メンバは、`static` を前に付けて定義する。


# 静的メンバ関数

特定のオブジェクトを作る処理を静的メンバ関数にするとよい。

というのも、静的メンバ関数もメンバなので作ったオブジェクトのメンバ変数をいじれるから。

```cpp
class Position {
  int _x, _y;
  bool _is_null;

  Position() : _x(0), _y(0), _is_null(false) {}
public:
  static Position coordinate(int x, int y) {
    Position p;
    p._x = x;
    p._y = y;
    return p;
  }

  static Position null() {
    Position p;
    p._is_null = true;
    return p;
  }
  // ...
};
```


# 静的メンバ変数

クラスのオブジェクト全体で共有する変数になる。

```cpp
class Wood {
  static std::string texturePath = "assets/wood_1.png";
};

// 静的メンバ変数はクラス内だと宣言になるので、定義が別に必要
// クラスにある名前は クラス名::メンバ名 になる
std::string Wood::texturePath = "assets/wood_1.png";
```

以下はシングルトンパターンと言う、オブジェクトが一つしか存在しないようにする書き方。

コピーコンストラクタを消して、勝手にコピーできないようにしている。

```cpp
class Singleton {
  static Singleton _instance;

  Singleton() {}
  Singleton(Singleton const&) = delete; // コピーコンストラクタを delete
  // ...
public:
  static Singleton& instance() { return _instance; }
  // ...
};
Singleton Singleton::_instance;
```

