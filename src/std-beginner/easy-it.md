# かんたんイテレータ

くるくるまわーる

---

**イテレータ** (iterator、反復子) は、コンテナの要素にアクセスするための、*コンテナクラス付属のクラス*。

コンテナクラスに対して、その要素を見る窓のようなクラスだよ。

このインスタンスに演算子を使うことで、

- 要素の参照を取得する (`*` 単項演算子)
- 次の要素に移動する (`++` / `+` 演算子)
- 前の要素に移動する (`--` / `-` 演算子)

などなどといったことができる。

とりあえず、実際にコンテナクラスのイテレータに触れていこう。


## std::vector::iterator

`std::vector` のイテレータクラスは `std::vector::iterator`。

最初の要素のイテレータは `begin`、末尾の更に後ろの存在しない要素は `end` で取得できる。

```cpp
std::vector<int> data = {1, 4, 5, 2};

std::vector<int>::iterator head = data.begin();

*head; // 1
++head;
*head; // 4
*head = 3;

std::vector<int>::iterator tail = data.end();

*tail; // これは末尾の次の要素なので不定
--tail;
*tail; // 2
--tail;
--tail;
*tail; // 3
```

いくつかのメンバ関数は、引数にイテレータを取る。

```cpp
std::vector<int> data = {4, 2, 2, 3};

// insert は イテレータ の位置以降を後ろにずらして、そこに 値 を入れる
data.insert(data.begin() + 1, 5); // 1 番目に 5 を挿入

data; // {4, 5, 2, 2, 3}

// erase は イテレータ の指す要素を削除して、それ以降を前に詰める
data.erase(data.begin() + 2); // 2 番目を削除

data; // {4, 5, 2, 3}

// erase は イテレータの範囲 (以上, 未満) を削除するものもある
data.erase(data.begin() + 2, data.begin() + 4); // 2 番目以上 4 番目未満を削除

data; // {4, 5}
```


## std::map::iterator

`std::map` のイテレータクラスは `std::map::iterator`。

最初の要素のイテレータは `begin`、末尾の更に後ろの存在しない要素は `end` で取得できる。

イテレータでアクセスする要素の順番は、*昇順* (小さい数から大きい数、文字列とかは辞書順) になっている。

なお、イテレータでアクセスした要素は、キーと値がセットになった `std::pair` クラスになっている。

このクラスからは、`first` / `second` メンバ変数でそれぞれ *キー* / *値* を取得できる。

```cpp
std::map<int, int> data = {
  {1, 4},
  {5, 2}
};

std::map<int, int>::iterator head = data.begin();

*head; // {1, 4}
++head;
*head; // {5, 2}
(*head).second = 3;

std::map<int, int>::iterator tail = data.end();

*tail; // これは末尾の次の要素なので不定
--tail;
*tail; // {5, 3}
--tail;
(*tail).first; // 1
```

イテレータを引数に取ったり、戻り値として返したりするメンバ関数はあるけれど、特に有用というわけでもないので紹介しないよ。


## イテレータと範囲 for

さて、久しぶりの *範囲 `for` 文* だよ。

実は、範囲 `for` 文はイテレータがあるものなら何でも使えるよ。

```cpp
std::vector<int> data = {1, 5, 2, 3, 4};
for (int element : data) {
  std::cout << element << "\n";
}
```

```cpp
std::cout << "値を何個入れるの? : ";
int input_n = 0;
std::cin >> input_n;

// input_n 個の入力を受付る
std::vector<double> input(input_n);
for (int &element : input) {
  std::cin >> element;
}
```

```cpp
std::map<std::string, int> score = {
  {"Ange", 510},
  {"Lize", 480},
  {"Toko", 540}
};

std::cout << "--- Score ---\n";
for (std::pair<std::string, int> element : score) {
  std::cout << element.first << ": "
    << element.second << " Pt.\n";
}
```

イテレータの大事さがわかったかな?

ちなみに、配列にイテレータがあるわけじゃないけど、特別に同じように扱えるようになっているよ。
