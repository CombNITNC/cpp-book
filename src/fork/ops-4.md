# 演算子 其の四

かつ、もしくは、ではない

---

`bool` 型どうしの演算もある。

値の種類が二つしかないから、全部のパターンを書いておくね。


## AND

`&&` は AND 論理積の演算子。

両者が `true` のときは `true` で、それ以外は `false` になる。

```cpp
false && false; // => false
true && false; // => false
false && true; // => false
true && true; // => true
```


## OR

`||` は OR 論理和の演算子。

どちらかが `true` であれば `true` で、それ以外は `false` になる。

```cpp
false || false; // => false
true || false; // => true
false || true; // => true
true || true; // => true
```


## NOT

`!` は NOT 否定の演算子。

`true` と `false` を逆にする。これだけは *値の頭につける演算子* だから注意。

```cpp
!true; // => false
!false; // => true
```


# 比較演算子との複合

`&&` や `||` は、`<` とかより優先順位が低いので、**条件の組み合わせ** が書ける。

```cpp
0 <= value && value <= 10; // value が 0 以上 10 以下のとき

value == 0 || value == 1 || value == 3; // value が 0, 1, 3 のとき

// この 3 つは同じ式。ド・モルガンの法則などによって変形してみた
!(5 <= value && value <= 7); // value が 5 以上 7 以下でないとき
!(5 <= value) || !(value <= 7); // value が 5 以上 7 以下でないとき
5 > value || value > 7; // value が 5 以上 7 以下でないとき
```

あんまり長いと見づらいので、そのときは `bool` 型の **変数に一旦保管** したほうがいいかも。