# アルゴリズム関数

てを　よこに　あら　あぶない　あたまを　さげれば　ぶつかりません

---

# algorithm

`algorithm` 標準ライブラリには、

イテレータと関数 (もっぱらラムダ式) を受け取って、

そのイテレータに処理を行う関数がたくさん用意されている。

ここでは、その中でよく使う関数を紹介していく。

全部見たい人は [ここ](https://cpprefjp.github.io/reference/algorithm.html) とか見るといいかも。

```cpp
#include <algorithm>
```

以下のコードを実行するときは、↑ が必要だよ。


## 交換 `swap`

2 つの引数の中身を入れ替える。

```cpp
int a = 0, b = 2;
std::swap(a, b); // a は 2、b は 0 になる

std::vector<int> data {2, 4}, buffer {5, 5};
std::swap(data, buffer); // vector のようなクラスでも対応しているので使える
```


## 最小と最大

### `min`

2 つの引数を比較して、小さい方を返す。

```cpp
std::min(2, 4); // 2
std::min(-4.5, -3.5); // -4.5
std::min(std::string("Vanessa"), std::string("Ivy")); // Ivy
```


### `min_element`

範囲の中を比較して最も小さいものを返す。

```cpp
std::vector<int> data {
  4, 8, -1, 2, 5, 5, 2, 3
};

std::min_element(data.begin(), data.end()); // -1
std::min_element(data.begin() + 3, data.end()); // 2
std::min_element(data.end() - 8, data.begin() + 2); // 4
```


### `max`

2 つの引数を比較して、大きい方を返す。

```cpp
std::min(2, 4); // 4
std::min(-4.5, -3.5); // -3.5
std::min(std::string("Vanessa"), std::string("Ivy")); // Vanessa
```


### `max_element`

範囲の中を比較して最も大きいものを返す。

```cpp
std::vector<int> data {
  4, 8, -1, 2, 5, 5, 2, 3
};

std::min_element(data.begin(), data.end()); // 8
std::min_element(data.begin() + 3, data.end()); // 5
std::min_element(data.end() - 8, data.begin() + 2); // 8
```


### `clamp`

最初の引数を、二番目の引数以上、三番目の引数以下に抑え込む。

```cpp
std::clamp(2, 5, 7); // 5
std::clamp(6, 5, 7); // 6
std::clamp(8, 5, 7); // 7

// これと同じ
std::min(std::max(2, 5), 7);
std::min(std::max(6, 5), 7);
std::min(std::max(8, 5), 7);
```


## 範囲探索

### `all_of`

範囲内の要素すべてが条件を満たすかどうか (三番目の引数の関数が実行して) 探索する。

結果がすべて `true` なら `true` を返し、そうでないなら `false` を返す。

```cpp
std::vector<int> data {
  4, 8, -1, 2, 5, 5, 2, 3
};

std::all_of(data.begin(), data.end(), [](int elem) {
  return -2 < elem;
}); // true
std::all_of(data.begin(), data.end(), [](int elem) {
  return elem == 2;
}); // false
std::all_of(data.begin(), data.end(), [](int elem) {
  return elem <= 4;
}); // false
```


### `any_of`

範囲内の要素のどれかが条件を満たすかどうか (三番目の引数の関数が実行して) 探索する。

結果がすべて `false` なら `false` を返し、そうでないなら `true` を返す。

```cpp
std::vector<int> data {
  4, 8, -1, 2, 5, 5, 2, 3
};

std::any_of(data.begin(), data.end(), [](int elem) {
  return -2 < elem;
}); // true
std::any_of(data.begin(), data.end(), [](int elem) {
  return elem == 2;
}); // true
std::any_of(data.begin(), data.end(), [](int elem) {
  return elem <= 4;
}); // true
```


### `find_if`

範囲内で条件 (第三引数) を満たす要素のイテレータを返す。

見つからなければ第二引数のイテレータを返す。

```cpp
std::vector<int> data {
  4, 8, -1, 2, 5, 5, 2, 3
};

std::find_if(data.begin(), data.end(), [](int elem) {
  return elem == 2;
}); // data.begin() + 3 と同じ
std::find_if(data.begin(), data.end(), [](int elem) {
  return elem < 0;
}); // data.begin() + 2 と同じ
std::find_if(data.begin(), data.end(), [](int elem) {
  return 10 <= elem;
}); // data.end() と同じ
```


### `count_if`

範囲内で条件 (第三引数) を満たす要素の数を返す。

```cpp
std::vector<int> data {
  4, 8, -1, 2, 5, 5, 2, 3
};

std::count_if(data.begin(), data.end(), [](int elem) {
  return elem == 2;
}); // 2
std::count_if(data.begin(), data.end(), [](int elem) {
  return elem < 0;
}); // 1
std::count_if(data.begin(), data.end(), [](int elem) {
  return 10 <= elem;
}); // 0
```


## 範囲操作

### `copy`

範囲を別の範囲へコピーする。

```cpp
std::vector<int> data(20), buffer(20);

// data.size() <= buffer.size() にしておくように
std::copy(data.begin(), data.end(), buffer.begin());
```


### `sort`

範囲を高速にソートする。第三引数に渡す関数で、並ぶ順序が変化する。

渡された関数を `op` とすると、`op(要素、次の要素)` が `true` になるようにソートする。

```cpp
std::vector<int> data {
  2, 5, 2, 5, 6, 2, 8, 1, 1, 4
};

std::sort(data.begin(), data.end());
// data は {1, 1, 2, 2, 2, 4, 5, 5, 6, 8} になる

std::sort(data.begin(), data.end(), [] (int a, int b) {
  return a > b;
});
// data は {8, 6, 5, 5, 4, 2, 2, 2, 1, 1} になる
```


### `unique`

隣り合った要素が重複しないように、範囲の先頭へ要素を集める。集めた後、新しい末尾のイテレータを返す。

ソート済みの場合は、独立した要素がすべて取り除かれることになる。

ソートしていない場合は、隣り合った要素は異なるようになる。

重複している要素を削除するわけではないよ。

```cpp
std::vector<int> data {
  2, 5, 2, 5, 6, 2, 8, 1, 1, 4
};

auto tail = std::unique(data.begin(), data.end());
// data は {2, 5, 2, 5, 6, 2, 8, 1, 4, 4}、tail は data.end() - 2 と同じ
```

```cpp
std::vector<int> data {
  2, 5, 2, 5, 6, 2, 8, 1, 1, 4
};

std::sort(data.begin(), data.end());
auto tail = std::unique(data.begin(), data.end());
// data は {1, 2, 4, 5, 6, 8, 5, 5, 6, 8}、tail は data.end() - 4 と同じ
```


## 二分探索

### `binary_search`

ソート済みの範囲に対して、二分探索を行う。

二分探索っていうのは、例えば五十音順に並んでいる辞書から探すときに、

1. 真ん中を開く。
2. 探しているものだったら終わり。
3. 探しているものがより前にあるなら、
   1. 前半分を見て、最初に戻る。
4. 探しているものがより後にあるなら、
   1. 後ろ半分を見て、最初に戻る。

という探し方。

つまり、順番通りに並んでいることを前提に、**前と後ろの二つに分けて探索する**。

`find_if` のように、`N` 個の範囲を一つづ探す場合は、それが探しているものかどうかを最低 `N` 回確かめる必要がある。

二分探索なら、これが `log2 N` 回確かめる (8 個なら 3 回、300 個なら 8 回) だけでよい。すごいね。

第三引数と等しい値が見つかった場合は `true` を、無いなら `false` を返す。

```cpp
std::vector<int> data {
  2, 4, 1, 2, 1, 7, 6, 8, 0
};

std::sort(data.begin(), data.end());

std::binary_search(data.begin(), data.end(), 6);
```

ソートを判断している関数が異なる場合は、その関数も第四引数に渡す必要がある。

```cpp
std::vector<int> data {
  2, 4, 1, 2, 1, 7, 6, 8, 0
};
auto greater = [] (int a, int b) { return a > b; };

std::sort(data.begin(), data.end(), greater);

std::binary_search(data.begin(), data.end(), 6, greater);
```

あ、ちゃんとソート済みの範囲を渡さないとちゃんと動かないよ。


# numeric

`numeric` 標準ライブラリには、整数計算用のアルゴリズム関数もある。

全部見たい人は [ここ](https://cpprefjp.github.io/reference/numeric.html) とか見るといいかも。


### `iota`

範囲を第三引数から始まる連番にする。

```cpp
std::vector<int> nums(20);
std::iota(nums.begin(), nums.end(), 0);
// nums は {0, 1, 2, 3, ..., 19}

std::vector<char> alphas(26);
std::iota(alphas.begin(), alphas.end(), 'A');
// alphas は {A, B, C, D, ..., Z}
```


### `accumulate`

範囲の要素と初期値を全部足した合計を返す。戻り値の型は第三引数の型と同じになる。

第四引数の関数で、足す以外の演算に変えることもできる。

```cpp
std::vector<int> data {
  2, 4, 1, 2, 1, 7, 6, 8, 0
};

auto sum = std::accumulate(data.begin(), data.end(), 0L); // long 型で 31
auto product = std::accumulate(data.begin(), data.end(), 1LL, [] (int a, int b) {
  return a * b;
}); // long long 型で 0
```


### `partial_sum`

各要素を前の要素からの合計にして、第三引数のイテレータへ順番に書き込む。↑ と同じく、最後の引数の関数で演算を変えることもできる。

```cpp
std::vector<int> data {
  1, 0, 0, -1, 2, 1, 0, -3, 0
};
std::vector<int> result(data.size());

std::partial_sum(data.begin(), data.end(), result.begin());
// result は {1, 1, 1, 0, 2, 3, 3, 0, 0} になる
```

ちょっと例題を解いてみよう。

以下は [こちら](https://imoz.jp/algorithms/imos_method.html) から引用。

> あなたは喫茶店を経営しています．
> 
> あなたの喫茶店を訪れたそれぞれのお客さん i (0≤i<C) について入店時刻 Si と出店時刻 Ei が与えられます（0≤Si<Ei≤T）．
> 同時刻にお店にいた客の数の最大値 M はいくつでしょうか．
> 
> ただし，同時刻に出店と入店がある場合は出店が先に行われるものとします．

入力の形式はこう。

```
C
S1, S2, ..., Si, ..., SC
E1, E2, ..., Ei, ..., EC
```

とりあえず、各時刻の客の数を格納して、客が滞在する時間をすべて累積していく方法でやってみる。

```cpp
#include <algorithm>
#include <iostream>
#include <vector>

int main() {
  using namespace std;
  int C, T;
  cin >> C >> T;
  vector<int> S(C), E(C);
  for (int &e : S) {
    cin >> e;
  }
  for (int &e : E) {
    cin >> e;
  }

  vector<int> customer_n(T);
  for (int customer = 0; customer < C; ++customer) {
    for (int time = S[customer]; time < E[customer]; ++time) {
      customer_n[time] += 1;
    }
  }
  cout << *std::max_element(customer_n.begin(), customer_n.end()) << "\n";
}
```

いもす法という解法でこれを解こうとすると、`partial_sum` が非常に役に立つ。

いもす法では、「滞在する時間」のような連続する状態を、始端を `+1`、終端を `-1` して客の出入りだけを格納するよ。

```cpp
std::vector<int> imos(T);
for (int customer = 0; customer < C; ++customer) {
  imos[S[customer]] += 1;
  imos[E[customer]] -= 1;
}
```

そして、`partial_sum` を書けると元のデータになるわけ。

```cpp
std::vector<int> customer_n(T);
std::partial_sum(imos.begin(), imos.end(), customer_n.begin());
```

コード全体はこう改善される。

```cpp
#include <algorithm>
#include <iostream>
#include <vector>
#include <numeric>

int main() {
  using namespace std;
  int C, T;
  cin >> C >> T;
  vector<int> S(C), E(C);
  for (int &e : S) {
    cin >> e;
  }
  for (int &e : E) {
    cin >> e;
  }

  vector<int> imos(T);
  for (int customer = 0; customer < C; ++customer) {
    imos[S[customer]] += 1;
    imos[E[customer]] -= 1;
  }
  vector<int> customer_n(T);
  partial_sum(imos.begin(), imos.end(), customer_n.begin());
  cout << *max_element(customer_n.begin(), customer_n.end()) << "\n";
}
```

最初のコードだと、C * T 回足さなきゃいけなかったけど、これは C + T 回足すだけでよくなってるよ。

いもす法を使った問題は、章末問題にもいくつか出すよ。
