# 名前空間とusing

:: をぶっ壊す!

---

もう気づいていると思うけれど、

今まで使ってきた標準ライブラリのものを見てみると、

`std::cout` とか `std::string` みたいに `std::` が付いてる。

これは、`std` という **名前空間** に入っているから付けなくちゃいけない。

`名前空間::名前` というふうに名前を使うようになっている。

自分が作った変数や関数と名前が被ってもいいようにしているんだ。

ここでは、名前空間の作り方は教えないよ。

その代わりに、何回も `std::` って書かなくていいようにする方法を紹介するね。

## using 宣言

`using 名前空間::名前;` と書くと、それ以降で **その名前を使うときは `名前空間::` を省略できる** ようになる。

```cpp
#include <iostream>
// ちなみに、iostream ファイルの中で #include <string> されている

int main() {
  std::cout << "Hi!\n";
  using std::cout;
  cout << "Shorter.\n";

  std::string text("Longer.");
  using std::string;
  cout << string("Short!");
}
```

## using ディレクティブ

`using namespace 名前空間;` と書くと、それ以降 **すべてで `名前空間::` を省略できる** ようになる。

ただ、予期しない名前被りを起こしやすいので使い所には注意。

```cpp
#include <iostream>

int main() {
  std::cout << "Hi!\n";
  using namespace std;
  cout << "Shorter.\n";

  std::string text("Longer.");

  cout << string("Short!");
}
```

こっちより using 宣言を使うべきだけど、競技プログラミングとかで少ないタイプにしたいなら使ってもいいよ。

```cpp
#include <iostream>

int main() {
  using namespace std;
  cout.tie(nullptr);
}
