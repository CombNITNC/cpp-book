# もっとイテレータ

それいけ!イテレータ

---

ここでは、まだ紹介しきれていなかったイテレータの性質について触れていくよ。


## イテレータからメンバへのアクセス

インスタンスをコンテナに格納すると、

そのイテレータからインスタンスにアクセスして、そこからメンバへアクセスすることが多くなる。

```cpp
std::vector<std::string> texts(10, "Hello");

auto it = texts.begin();

(*it).at(3) = 'i';
++it;

(*it).size();
++it;

(*it).substr(1, 2);
```

「イテレータ→インスタンス→メンバ」を「イテレータ→メンバ」と書ける演算子として、`->` (矢印とかアローとか読む) 演算子がある。

```cpp
std::vector<std::string> texts(10, "Hello");

auto it = texts.begin();

it->at(3) = 'i';
++it;

it->size();
++it;

it->substr(1, 2);
```


## イテレータの距離

`std::distance` 関数は、同じコンテナに対するイテレータの距離を求めることができる。

```cpp
std::vector<int> data {2, 1, 1, 4};

std::distance(data.begin(), data.end()); // 4

auto it = data.begin();
++it;
++it;
std::distance(it, data.begin()); // -2
```


## イテレータの比較

イテレータにはいくつか種類があるけれど、どんなイテレータでも `==` と `!=` 演算子で同じかどうかを比較できる。

```cpp
std::vector<int> data {2};

auto it = data.begin();

it == data.begin(); // true
it != data.end(); // true

++it;

it == data.begin(); // false
it != data.end(); // false
```

## begin / end 関数

どんな型に対しても、`std::begin` / `std::end` 関数 (メンバ関数ではなく普通の関数) に値を渡すとその始端/終端を取得できる。

```cpp
int array[3] {2, 4, 5}; // 配列はクラスではないので、メンバ関数はない

auto it = std::begin(array); // しかしイテレータ相当のものはある

*it; // 2
++it;

*it = 3;
it += 2;

it == std::end(array); // true
```


## イテレータの種類

標準ライブラリだけでも、それなりのイテレータクラスがある。それらを分類する呼び方があるので、例を示しながら紹介しておく。


### 出力イテレータ

* 値を書き込める
* 進める (それ以前の要素は無効になる)

ようなイテレータのこと。

```cpp
*it = 2;
auto tmp = it;
++it; // tmp のイテレータは要素にアクセスできなくなる
```

`std::ostream_iterator`、`std::back_insert_iterator` などが該当する。

```cpp
// 出力ストリーム (std::cout など) を出力イテレータにするクラス
// テンプレート引数に出力する型が必要
std::ostream_iterator<int> out(std::cout);

*out = 2; // std::cout << 2; と同じ
*out = 3; // std::cout << 2; と同じ

++out; // インクリメントしても何も起きない

// 第二引数に文字列を渡すと、書き込むときにそれも流す
std::ostream_iterator<int> out_line(std::cout, ",\n");

*out_line = 4; // std::cout << 4 << ",\n"; と同じ


std::vector<int> data {4, 3, 5, 5, 1, 2};

std::copy(data.begin(), data.end(),
  std::ostream_iterator<int>(std::cout, ", ")
); // らくらく出力
```

```cpp
std::vector<int> data;
auto in = std::back_inserter(data); // back_insert_iterator を作る関数

// back_insert_iterator は、代入するとそれが push_back される
*in = 2; // data は {2}
*in = 4; // data は {2, 4}

++in; // インクリメントしても何も起こらない
```


### 入力イテレータ (InputIterator)

* 値を読み出せる
* 進める (それ以前の要素は無効になる)

ようなイテレータのこと。

```cpp
auto instance = *it;
it->m; // メンバーへのアクセスも値の読み出し
auto tmp = it;
++it; // tmp の中身は無効になる
```

`std::istream_iterator` や、だいたいのコンテナのイテレータが該当する。

```cpp
std::istream_iterator<int> in(std::cin);

int A = *in;
int B = *in;
// A == B になる

++in; // インクリメントで次の入力を読み込む

int C = *in;

++in; // もう入力がない場合は無効なイテレータになる
```

```cpp
int N;
std:cin >> N;

std::vector<int> data;
data.reserve(N); // 要素は作らないが事前にメモリを確保するメンバ関数
// ↑ で後々の back_insert_iterator による push_back が高速化される

auto data_in = std::back_inserter(data);

std::istream_iterator<int> in(std::cin), nullin; // 引数無しで作った nullin は最初から無効なイテレータ

// ↓ は in == nullin になるまで動く
std::copy(in, nullin, data_in); // らくらく入力
```


### 前方向イテレータ

* 値を読み出せる
* 値を書き込める (const 型のイテレータの場合はできない)
* 進める
* 等しいかどうか他のイテレータと比較できる

ようなイテレータのこと。

```cpp
auto data = *it;
*it = 1;
auto tmp = it;
++it;
tmp == it; // false
```

入力イテレータもこれに含まれる。書き込める場合は出力イテレータもこれに含まれる。

`std::list::iterator` などがこれにあたる。まず使わないけど (例も出さない)。


### 双方向イテレータ

* 値を読み出せる
* 値を書き込める (const 型のイテレータの場合はできない)
* 進める
* 戻れる
* 等しいかどうか他のイテレータと比較できる

ようなイテレータのこと。

前方向イテレータもこれに含まれる。

`std::deque::iterator` などがこれにあたる。

```cpp
// std::deque は std::queue の中身などに使われている
std::deque<int> d {4, 6, 5, 2, 2, 1};
auto it = d.begin();
++it;
*it = 3;
--it;
*it; // 4
it == d.begin(); // true
```


### ランダムアクセスイテレータ

* 値を読み出せる
* 値を書き込める (const 型のイテレータの場合はできない)
* 進める
* 戻れる
* 繰り返しを使わずに任意の位置に移動できる
* 整数で任意の位置に移動してアクセスできる
* 他のイテレータとあらゆる比較ができる

ようなイテレータのこと。

双方向イテレータもこれに含まれる。

`std::vector::iterator` などがこれにあたる。

```cpp
std::vector<int> d {3, 1, 5, 3, 1};
auto it = d.begin();
++it;
*it; // 1
--it;
*it; // 3
it += 4;
*it = 2;
it -= 2;
d.begin() < it; // true
it <= d.end(); // true
it == d.end(); // false
it != d.begin(); // true

it[2]; // 2。*(it + 2); と同じ
it[-2]; // 3。*(it - 2); と同じ
```

---

こういったイテレータの分類は、今後の説明でちょいちょい使うよ。すぐには覚えなくていいからね。
