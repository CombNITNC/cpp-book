# break

全て壊すんだ

---

`continue` に続いて、`break` もループ制御する文だよ。

そういえば `switch` 文のときにもでてきてたね。

これは、直近の **ループを終了する文** だよ。

実際に使うとこんな感じ。

```cpp
#include <iostream>
int main() {
  int count = 0;
  while (true) {
    std::cout << "カウント: " << count << "\n"
     << "1 でカウントアップ | それ以外で終了\n";

    int input;
    std::cin >> input;
    if (input ！= 1) { break; }
    ++count;
  }
}
```

繰り返しをとりあえず無限ループにしておいて、終了を `if` と `break` にするのはよくやる。

```cpp
bool should_exit = false;
while (true) {
  if (should_exit) { break; }
  // :
  // :
}
```

`switch` 文でも、文の中から抜け出すときに使われているけど、別物として覚えたほうがいいよ。
