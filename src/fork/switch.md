# switch

タコ足配線

---

整数で分岐するときに、このように **等しいかどうかの分岐を連ねる** ということをやったりする。

```cpp
std::cout << "Select:\n  1. Hello.\n 2. Hi！\n other. ？？？\n";

int select;

std::cin >> select;

if (select == 1) {
  std::cout << "Hello.\n";
} else if (select == 2) {
  std::cout << "Hi！\n";
} else {
  std::cout << "？？？\n";
}
```

これは `switch` 文で書き換えられる。文法は以下の通り。

> switch ( *整数の式* ) {
> 
> case *値*:
> 
>   *式と値が等しいときの文*
> 
>   break;
> 
>   :
> 
>   :
> 
> default:
> 
>   *どの case にも当てはまらないときの文*
> 
>   break;
> 
> }

すると、*case に一致する値のところ* まで、**実行する文が飛ばされる**。

`case 値` の後ろに付いているのは、`;` ではなく `:` だよ。間違えないように。


最初の例を `switch` に書き換えるとこうなる。どっちが見やすいと思う？どっちでもいいけど。

```cpp
std::cout << "Select:\n  1. Hello.\n 2. Hi！\n other. ？？？\n";

int select;

std::cin >> select;

switch (select) {
case 1:
  std::cout << "Hello.\n";
  break;
case 2:
  std::cout << "Hi！\n";
  break;
default:
  std::cout << "？？？\n";
  break;
}
```

`switch` 文は整数にしか使えない！ ...そういう仕様なんだ.

この `break` (処理を脱出する特殊な文) が無いときは、そのまま下へ処理が流れていく。`break` を忘れないように。

```cpp
switch (input) {
case 0: {
    // input が 0 のとき
  }
  // break しないと……
case 1: {
    // input が 0 か 1 のとき
  }
  break;
default:
  // それ以外のとき
  break;
}
```

`break` の解説は次の章でやる。

これは本来の使い方ってわけでもないからね。
