# 多重コンテナ

コンテナを開けると、そこはコンテナだった

---

ここでは、`vector` の中に `vector` を入れるのをやっていくよ。

`std::vector<std::vector<int>>`

このクラスを初期化するときは、こうなるよ。

```cpp
std::vector<std::vector<int>> matrix(2, // 2 個の ↓
  std::vector<int>(2, 1) // 2 個の 1
);
```

アクセスするときも、`at` が 2 個出てくる。

```cpp
std::vector<std::vector<int>> matrix(2,
  std::vector<int>(2, 1)
);

matrix.at(0).at(0); // 1
```


## 使い道
