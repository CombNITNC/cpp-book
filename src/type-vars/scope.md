# スコープ

変数はいつか死ぬ

---

変数は、名前の宣言があってから、その複文の終わりまでの間だけで使用できる。

```cpp
int main() {
  // まだ何も使えない
 
  wide; // wide は使えないので、コンパイルできない
 
  int wide; // wide が使えるようになる

  { // スコープの始まり
 
    int narrow; // narrow 生誕
    narrow = 1;
    wide = narrow; // スコープの外側も普通に使える

  } // narrow 死す

  wide; // 1

  {

    int narrow; // narrow 生誕
    // 上の narrow とは別の変数、偶然に同名なだけ
    narrow = wide + 1;
    wide = narrow;

  }

  wide; // 2

  narrow; // narrow は使えないので、コンパイルできない
} // wide が使えなくなる
```

これを **スコープ** という。

つまり、複文を使うと変数の生存期間を限定できる。


# 名前が隠れる

ネストした複文の外側と内側で同名の名前がある場合は、**内側のものが優先** される。

```cpp
{
  int num; // A
  num = 0;
  {
    int num; // B
    num = 3; // これは B
  }
  num; // 0。これは A
}
```

ただし、読みにくくなるので違う名前をつけるべき。
