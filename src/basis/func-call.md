# 文と関数呼び出し

晩ご飯ですよ〜！

---

**文** は、処理を書くためのもの。

```cpp
;
```

これは何もしない文で、空 (から) 文とかヌル文とかいう。あんまり使わないけど。この `;` は文章の「。」みたいなもので、これが文の終わりになる。`;` (セミコロン) と `:` (コロン) を間違えないように。私もよく間違える。

文を複数連ねて書くと、**順番に実行** されていく。

```cpp
; // 文1
//   ↓
; // 文2
//   ↓
; // 文3
```

この文を main 関数の中に 0 個以上、複数書いていく。

```cpp
int main() {
  ; // 文1
  ; // 文2
  ; // 文3
}
```

さて、「何もしない」を学んで終わりじゃないよ。

**関数** というものがある。これは誰かさんが作ったプログラム。これを `関数名 ( 渡すデータ )` と書くと関数を使える。

なぜかみんな忘れるんだけど、***関数の呼び出しは 関数名 と 括弧*** だよ！

```cpp
#include <cstdio> // この中に std::printf 関数のプログラムが入っている
int main() {
  std::printf(" あなたのすきなことばを "); // テキストは「"」で囲って書く
  std::printf(" いくらでも書ける ");
  std::printf(" すごい(実際すごい) ");
}
```

これを実行すると、どうなるかな？

これを関数を **呼ぶ** とか **呼び出す** とかいう。今回は `std::printf` 関数を呼んでみた。

いっしょにテキストを渡しているけれど、これを自由に書き換えてみて。黒い画面に **表示されるテキストが変わる** はず。


ちなみに、この `#include <~>` は、指定したファイル (ここでは `cstdio`) をドバっと展開する。

関数を呼び出すことで、世界中のいろんな人たちが作ったプログラムを使える。すごい？……たぶんすごい！


`main` 関数も関数の一つだけど、一個しか存在できないし、呼び出すことができないようになってる (実は C 言語だと呼び出せる)。
