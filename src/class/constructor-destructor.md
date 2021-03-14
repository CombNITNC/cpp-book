# コンストラクタ/デストラクタ

トラクター

---

クラスには **コンストラクタ** と **デストラクタ** という *特殊なメンバ関数* を定義できる。


# コンストラクタ

**オブジェクトを作るときに自動で呼ばれる** メンバ関数で、**メンバ変数の初期化** とかをする。

```cpp
struct Cons {
  Cons(); // コンストラクは、クラス名と同じ名前で戻り値型を書かない
};
Cons c; // Cons のコンストラクタが呼ばれる。
```

引数のあるコンストラクタも作れる。

逆に、引数が必要ないコンストラクタを *デフォルトコンストラクタ* って言う。

```cpp
struct Cons {
  Cons(int) {}
};
Cons a(1), b{2}, c = 3;
```

逆に、コンストラクタを `private` にしちゃうとクラスのメンバからしかクラスのオブジェクトを構築できなくなる。

```cpp
class CannotCons {
  CannotCons() {}
};
CannotCons c; // エラー
```

このテクニックの出番は次のページだよ。


## コピーコンストラクタ

クラスのオブジェクトを変数から変数へ *コピーするときに呼ばれるコンストラクタ* を定義できる。

自身と同じ型の `const &` を受け取るコンストラクタがコピーコンストラクタになる。

```cpp
#include <iostream>
struct SayCopy {
  SayCopy() {}
  SayCopy(SayCopy const&) {
    std::cout << "コピーしたぜ!\n";
  }
};
SayCopy a;
SayCopy b = a; // コピーしたぜ!
```

定義の部分を `= default` にすれば、自動で全メンバ変数をコピーする処理にしてくれる。

```cpp
struct AutoCopy {
  AutoCopy() {}
  AutoCopy(AutoCopy const&) = default;
};
```

> ムーブコンストラクタもあるけど、それはまたの機会に。


## 初期化子

コンストラクタの引数リストとブロックの間で、メンバ変数を初期化する構文がある。

コンストラクタの引数でメンバ変数を初期化するときに便利。

```cpp
class Position {
  int _x, _y;

public:
  Position() : _x(0), _y(0) {}

  // : の後に メンバ変数名(初期値) をカンマ区切りで書く
  Position(int x, int y) : _x(x), _y(y) {}
};
```


## 初期化リスト

`std::initializer_list` というものをコンストラクタの引数で受け取るようにすると、初期化リストに対応したクラスが作れる。

```cpp
#include <vector>

class IntList {
  std::vector<int> vec;
public:
  IntList(std::initializer_list<int> list) : vec(list) {}
};

IntList list {1, 4, 2, 3, 5, 2, 3};
```


# デストラクタ

**オブジェクトが使えなくなるときに自動で呼ばれる** メンバ関数で、**リソースの解放** とかをする。

大抵は書かなくて良いと思うけど、コンストラクタとかでやった処理の後始末が必要なときにはここでやろう。

```cpp
#include <cstdio>

class Des {
  FILE *handler;

  Des() {
    // ファイルを開く fopen 関数
    handler = std::fopen("nanntoka.txt", "r");
    // ファイル操作した後は fclose を呼んでファイルを閉じるべき
  }
  ~Des() {
    // この Des が破棄されるとファイルも閉じられる
    std::fclose(handler);
  }
};

{
  Des des; // ファイルが開かれる
} // ファイルが閉じられる
```
