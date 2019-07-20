# long short signed unsigned

長いの短いの付いてないの

---

今回も、ちょっとマイナー寄りな感じ。

型には、指定子というものをつけて型の性質を少し変化させることができるよ。


# サイズを変える指定子

## long

`long` は、それなりによく使う指定子。型の扱えるサイズが元のサイズ以上になる。

だいたいの環境だと、`int` は `±2 の 31 乗 (約21億)` まで扱えるんだけれど、`long int` は `±2 の 63 乗 (約922京)` まで扱えちゃう。

前にやった *階乗の計算* みたいな、**大きい数値の計算だと必須**。

リテラルの後ろに `l` か `L` をつけると、`long` なリテラルになる。

```cpp
int normal = 21034;
long int big = 64683521l;
int long huge = 1029482019312L;

// long だけだと long int とみなされる
long large = 5l;
```

あと、`double` にも `long` をつけられる。

`double` は `約 ±1.79 × 10 の 308 乗` まで扱えるけれど、`long double` は `約 ±1.18 × 10 の 4932 乗` まで扱える。すごいけど、環境によっては動かないこともあるので注意。

```cpp
double normal = 1e8;
long double big = 1e60l;
double long huge = 1e1200L;
```

`long` をつけられるのは、`int` と `double` だけ。


## short

これは本当にめったに使わない。型の扱えるサイズが元のサイズ以下になる。

だいたいの環境だと、`-32768` ~ `32767` まで扱える。

`short` にリテラル用の記法は無い。悲しいね。

```cpp
int normal = 12000;
short int small = 21;
int short tiny = 3;

// short だけだと shor int とみなされる
short bit = 1;
```


## long long

頭悪い書き方だけど、`long` 以上のサイズになる `long long` もある。これも環境によっては動かない。

だいたいの環境だと、`約 ±9.22 × 10 の 18 乗` まで扱える。

リテラルの後ろに `ll` か `LL` をつけると、`long` なリテラルになる。

```cpp
long long int big = 20000000000LL;
long int long huge = 20000000000LL;
int long long scaled = 20000000000LL;

// long long だけだと long long int とみなされる
long long large = 1;
```


# 符号を操作する指定子

## unsigned

`unsigned` (日本語で符号なし) は、型が **プラスしか表現できなくなる**。*整数型にだけ* 付けられる。

その代わりに、マイナスに使っていたサイズがプラスに使われるので、二倍の数値が扱える。

……まぁ二倍くらいの差しかないので、なにか別のこと (後の章でやるビット演算とか) くらいにしか使わない。

```cpp
unsigned char a;
unsigned short b;
unsigned int c;
unsigned long d;
unsigned long long e;
```


## signed

`signed` (日本語で符号あり) は、普通のときと変わらない。指定はしなくてもいい。

```cpp
signed int a;
```

実は `char` が `signed` なのか `unsigned` なのかは *環境によって変わる* ので、そのへんを厳密にしたいときには使う。

```cpp
signed char a = 127;
unsigned char b = 255;
```
