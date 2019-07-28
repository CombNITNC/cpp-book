# コンテナ

みんなコンテナは持ったな!!

---

今までの入力では、繰り返したとしても一つずつしか扱えなかった。

```cpp
#include <iostream>

double add(double a, double b) {
  return a + b;
}

void ask_sum() {
  using std::cin;
  using std::cout;

  cout << "合計を出力するべ。\n"
    << "値をスペースとかで区切って 2 つ入力してくれ。\n";
  double first = 0, second = 0;
  cin >> first >> second;

  // 入力エラー処理テンプレ
  while (cin.fail()) {
    cin.clear();
    cin.ignore();
    cout << "なんかおかしいべ。もう一度入力してけろ。\n";
    cin >> first >> second;
  }

  cout << "合計 = " << add(first, second) << "\n";
}

int main() {
  while (true) {
    ask_sum();

    std::cout << "もうやめるかい? [y で終了]";
    char command;
    std::cin >> command;
    if (command == 'y' || command == 'Y') {
      break;
    }
  }
}
```

これだと、大量のデータを集計して平均を求めたりとかができない。

そこで、そういうのを扱う **コンテナクラス** を使う。

**コンテナ** っていうのは、いくらでも値を詰め込めるクラスのこと。

`std::string` もコンテナの一種といえるよ。

よく使う `std::vector` から見ていこう。


## std::vector

同じ型のデータを大量に格納するクラス。サイズを増やせる配列みたいな感じ。

使うには、`#include <vector>` が必要。

```cpp
#include <vector>

int main() {
  std::vector<int> data;
}
```

これは *テンプレート* といって、使う側から型や値を *テンプレート引数* として渡せる仕組み。

> *テンプレート名*  < *テンプレート引数*, ... >

ここでは格納する型を渡すと、その型が格納できるクラスになる。

つまり、

* `std::vector<int>` は `int` が格納できるクラス
* `std::vector<double>` は `double` が格納できるクラス

ってこと。



### 末尾へ追加

`push_back` メンバ関数は、リストの後ろに値を一つ追加する。`std::vector` が最も得意な操作。

```cpp
std::vector<int> data;

data.push_back(4);
data.push_back(3);
data.push_back(1);
data.push_back(0);
data.push_back(5);
```


### 初期化

このクラスの初期化は二種類ある。


#### サイズ指定

予め必要なサイズとその中を埋める値を指定するやり方。

```cpp
std::vector<int> stat(20); // 20 個の 0
std::vector<double> data(15, 5.0); // 15 個の 5.0
```

すべて同じ値にすることしかできないので、`0, 1, 2, 3, ...` みたくするときは `for` で要素を追加していく必要がある。

```cpp
std::vector<int> nums;

for (int i = 0; i < 20; ++i) {
  nums.push_back(i);
}
```


#### リスト

中に格納するデータをそのまま書く。

```cpp
std::vector<int> stat = {2, 4, 0, 3};
std::vector<double> data = {1.0};
```


### 取得

丹精込めて詰め込んだ値を取り出そう。


#### サイズ

`size` メンバ関数で詰め込んだ値の数を取得できる。

```cpp
// 何も入れていないと 0
std::vector<int>().size(); // 0

std::vector<int> data(4);
data.size(); // 4
```


#### 要素

`at` メンバ関数で要素の参照を取得できる。

配列と同じで最初の要素は `0` 番目。

```cpp
std::vector<int> data = {4, 3, 1, 0, 5};

data.at(1); // 3

data.at(4) = 2; // 変更もできる

data.at(6); // 存在しない要素。エラーで止まる
```


#### 末尾要素

```cpp
std::vector<int> data = {2, 4, 5};
```

↑ の最後の要素にアクセスするときは、

```cpp
data.at(data.size() - 1);
```

よりも、

```cpp
data.back();
```

でできる。


### 末尾から削除

`pop_back` で、リストの後ろから値を一つ削除する。

```cpp
std::vector<int> data = {4, 6, 8};

data.pop_back();
```


## std::map

`std::map` を使うには、`#include <map>` が必要。

これは、値と、それを表すキー値、二つのセットをたくさん格納するクラス。

キーの値を渡せば値が取得できるってしくみ。

その性質上、キーが重複するような格納はできない。

ちなみに、よく連想配列とか辞書配列とか言われる。

```cpp
std::map<std::string, int> score;
```

1 つ目のテンプレート引数が *キーの型* で、2 つ目のテンプレート引数が *対応する値の型*。

↑ の型は、`std::string` をキーとして `int` を格納するクラス。


### 初期化 

このクラスにはリスト初期化しかない。

```cpp
std::map<int, int> integer = { // 初期化リストの中に
  {0, 1}, // 初期化リストたち
  {1, 2},
  {2, 3}
};

std::map<std::string, int> score = {
  {"Ange", 510},
  {"Lize", 480},
  {"Toko", 540}
};
```


### 取得

取得には二通りの方法があって、それぞれ性質が異なる。


#### サイズ

こっちも、格納している要素の数は `size` で取得できる。

```cpp
std::map<int, int> data = {
  {0, 1},
  {1, 2},
  {2, 3}
};

data.size(); // 3
```


#### []

配列のように `[ キー ]` で取得できる。

まだそのキーの値がないときは、`0` や空で初期化されたものが入る。

```cpp
std::map<int, int> matrix = {{1, 2}};

matrix[1]; // 2

matrix[3]; // 無いので追加。0

matrix[-1] = 4; // 無いので追加。そして代入


std::map<std::string, std::string> word;

word["Hello"]; // 無いので追加。std::string() が入る

word["Good morning"] = "こんばんは"; // 文字列が std::string になって入る
```

こんな感じで、要素の挿入もこれでできちゃう。


#### at

`at` も取得するメンバ関数。

`std::vector` のように、存在しないときはエラーで止まる。

```cpp
std::map<char, int> hex = {
  {'A', 10},
  {'B', 11}
};

hex.at('A'); // 10
hex.at('C'); // エラーで止まる
```

### 要素の確認

`contains` は指定のキーが格納されているかどうかを `bool` で返す。

```cpp
std::map<char, int> hex = {
  {'D', 13}
};

bool has_a = hex.contains('A');
bool has_d = hex.contains('D');
```

要素があるときだけその要素を取得したいときは、上の `at` と組み合わせる。

```cpp
std::map<char, int> hex = {
  {'D', 13}
};

bool has_a = hex.contains('A');
if (has_a) {
  int &a = hex.at('A');

}

bool has_d = hex.contains('D');
if (has_d) {
  int &d = hex.at('D');

}
```

### 削除

`erase` で指定したキーの要素を削除する。

```cpp
std::map<std::string, long> record = {
  {"0d5e3", 4000},
  {"4ghd7", 8002},
  {"5hf71", 12032},
};

record.erase("0d5e3");
```

---

他にもいろんなコンテナクラスやそのメンバ関数がある。

けど、その前に次のページで **イテレータ** について軽く触れておこう。
