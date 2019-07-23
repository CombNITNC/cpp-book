# 範囲for文

もっと楽に回る

---

ここで、非常に便利な制御文を紹介するよ。

C++ やるなら、これが使えるとすごく楽になるからね。

## 文法

**範囲 for 文** は、データに対する繰り返しが楽にかける `for` だよ。

> for ( *変数* : *反復するもの* ) *繰り返す文*

これは、

1. *反復するもの* から値を一つづつ取り出して変数の中に入れる。
2. *繰り返す文* を実行する。↑ の変数が使える。

というもの。

## 実例

実際に使うとこんな感じ。

まず、配列に使える。

```cpp
int nums[] = {4, 2, 3};

for (int element : nums) { // コピーで取り出して、
  std::cout << element << " "; // 一つづつ出力
}

for (int &element : nums) { // 参照で取り出して、
  --element; // 一つ減らす
}

// もう一度出力
for (int element : nums) {
  std::cout << element << " ";
}
```

`std::string` にも (クラス側が対応しているから) 使える。

```cpp
std::string text("Hello");

for (char part : text) { // コピーで取り出しても、
  ++part; // 変更は反映されない
}

std::cout << text << "\n";

for (char &part : text) { // 参照で取り出すことで、
  ++part; // 変更できる
}

std::cout << text << "\n";
```
