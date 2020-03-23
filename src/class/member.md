# メンバ定義

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
Position p; // p は x、y、norm とかを持っている
```


# メンバ関数

メンバ関数を定義すると、クラスのオブジェクトに関数がくっつく (厳密には所有じゃない)。

```cpp
class Position {
  void add(Position const&) { /* ... */ }
  double abs() const {// ← の const 修飾をつけると、オブジェクトが const のときだけ呼び出せる
   // ...
  }
  :
  :
};
Position p; // p に add、norm などの関数がくっついている
```


# アクセス指定子 public / private

メンバには公開と非公開があって、公開されているメンバしか外部から触れない。

それぞれ、**アクセス指定** をすることで設定できる。


## public

`public` と指定したメンバのみ、クラスの外からアクセスできる。

```cpp
class Counter {
  // ...
public: // これ以降で定義したメンバは全て public になる
  void add(int n) {
    safeAdd(n); // 後述の private なメンバ関数
  }
  int getCount() const { // メンバ変数を変えない場合は const を付けるのが C++er
     return count;
  } 
};
Counter c;
c.add(2);
auto const count = c.getCount();
```

`struct` で作ったクラスのデフォルトの指定子でもある。

```cpp
struct Chunk {
  int a;
  int b;
};
Chunk c;
c.a = 2;
auto b = c.b;
```


## private

`private` と指定したメンバは、同じクラスのメンバからしかアクセスできない。

```cpp
class Counter {
private: // これ以降で定義したメンバは全て private になる
  int count;

  void safeAdd(int n) {
    if (0 < n && count + n < 100) {
      count += n;
    }
  }
  // ...
};
```

メンバは基本的にこの指定子にする。メンバ関数が少ない方が分かりやすいからね。

`class` で作ったクラスのデフォルトの指定子でもある。

> 本当はもう一つアクセス指定子があるんだけど、それは無くてもいいのでまた別の機会に

**メンバ変数はできる限り `private` に** して、その変更はメンバ関数で行うこと。
クラスのメリットの一つに、**メンバ変数の値をメンバ関数内で正常なままに制御できる** って所があるからね。
メンバ変数をいじるときは、**メンバ関数を通してそこから変更する** ようにしてね。
