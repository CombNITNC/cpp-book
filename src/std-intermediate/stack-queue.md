# スタックとキュー

L! I! F! O!

---

ここでは、ちょっと変わったコンテナとその使い道を紹介していく。

## stack

データを後ろからのみ入れることができて、後ろから逆の順序でしか取り出せないコンテナ。

データを皿のように積み上げていくイメージ。

ここでのスタックは MTG の用語じゃないよ。

```
↑ 取ったり ↓ 乗せたり
ーーー
| 2 |
ーーー
| 0 | こことか
ーーー
| 1 | ここは取り出せない
ーーー
```

`#include <stack>` が必要。

```cpp
// vector と同じで格納する型はテンプレート引数
std::stack<int> S;

// push でスタックに一つ「積む」
S.push(2); // {2}

// pop でスタックから一つ取り除く
S.pop(); // { }

S.push(4); // {4}
S.push(3); // {4, 3}
S.pop(); // {4}
S.push(1); // {4, 1}

// empty で空かどうかを bool で取得
while (!S.empty()) {
  // top でスタックの一番上 (後ろ) を取得
  auto top = S.top();
  S.pop();
  std::cout << top << " ";
}
```

これは、やることを積んでおいて逆順に解決していくのに使うよ。


### 例題

例題をやってみる。

> 入力:
> ```
> N
> T
> ```
> - `N` は `0` 以上 `T の長さ` 未満の整数
> - `T` は文字列
> 
> `'('` と `')'` で構成された文字列が与えられる。
>
> `'('` と `')'` は必ず対応付け (開いて閉じること) がされている。
>
> 文字列 `T` の `N` 番目の位置 (0 始まり) の  `'('` または `')'` に対応している、反対向きの括弧の位置 (0 始まり) を探す。
> 
> その位置を出力せよ。

例えば、入力が

```
5
((())(()()))(())
```

の場合は、

```
( ( ( ) ) ( ( ) ( ) ) )  ( (  ) )
0                     11 12     15
  1     4                  13 14
    2 3
          5         10
            6 7
                8 9
```

と対応付けられるので、`10` を出力する。

1. 文字列の中の文字を一つづつ見る
   1. 括弧が始まっている
      1. その位置を `stack` に乗せて次の文字へ
   2. `stack` の `top`、もしくは今見ている位置が `N` に等しいかどうか見る
      1. 等しければ反対側にあたる位置を出力して終了
   3. 括弧が終わっている (他の文字がないので常に `true`)
      1. `stack` を一つ取る

と解けば楽ちん。

```cpp
#include <iostream>
#include <stack>

int main() {
  int N;
  std::string T;
  std::cin >> N >> T;

  std::stack<int> brace_index;
  for (size_t i = 0; i < T.size(); ++i) {
    if (T[i] == '(') {
      brace_index.push(i);
      continue;
    }
    if (i == N) {
      std::cout << brace_index.top() << "\n";
      break;
    }
    if (brace_index.top() == N) {
      std::cout << i << "\n";
      break;
    }
    brace_index.pop();
  }
}
```


## queue

データを入れると、入れた順番でしか取り出せないコンテナ。

要素は後ろから追加する。

要素の取得と削除は前側からしかできない。

`#include <queue>` が必要。

```cpp
std::queue<int> Q;

// push と pop があるけど stack とは違う

Q.push(2); // {2}
Q.pop(); // { }
Q.push(4); // {4}
Q.push(3); // {4, 3}
Q.pop(); // {3}
Q.push(1); // {3, 1}

// empty で空かどうかを bool で取得
while (!Q.empty()) {
  // front でキューの先頭を取得
  auto head = Q.front();
  Q.pop();
  std::cout << head << " ";
}
```


### 例題

それじゃあ毎度おなじみ例題だよ。

> 入力:
> ```
> V E S G
> src1 dst1
> src2 dst2
> :
> srcE dstE
> ```
> - `V` と `E` は正の整数
> - `S` と `G` は `0` 以上 `E` 未満の整数
> - `srcE` と `dstE` は `0` 以上 `V` 未満の整数
> 
> `V` 個の異空間がある。
>
> 異空間どうしの間には二つの空間をつなぐ一方通行のポータルがあり、`src` 番目の空間から `dst` 番目の空間に向かってだけ移動できる。
> 
> ポータルを利用して、`S` 番目の空間から `G` 番目の空間へ移動できるかどうか求めよ。
>
> 移動できる場合は `"YES"`、できない場合は `"NO"` を改行を加えて出力せよ。
> 
> ---
> 入力例1:
> ```
> 5 8 0 4
> 0 1
> 1 2
> 2 3
> 3 4
> 0 2
> 0 3
> 1 0
> 1 3
> ```
> 出力例1:
> ```
> YES
> ```
> ---
> 入力例2:
> ```
> 3 4 0 1
> 0 2
> 0 2
> 2 0
> 2 2
> ```
> 出力例2:
> ```
> NO
> ```

それじゃあ入力を受け付けよう。

```cpp
using std::cin;
int V, E, S, G;
cin >> V >> E >> S >> G;

// adj[i] を i 番目の空間から入れるポータルの行き先リストとする
std::vector<std::vector<size_t>> adj(V);
for (int i = 0; i < E; ++i) {
  int src, dst;
  cin >> src >> dst;
  adj[src].push_back(dst);
}
```

これでデータは用意できた。あとは、

1. 最初の空間である `S` をキューに `push`
2. キューが空になるまで繰り返し
   1. キューの `front` を `now` として取り出し、`pop`
   2. `now` が `G` なら
      1. 到達可能なので終了
   3. `now` が *通過済み* なら次のキューの要素へ飛ばす
   4. `now` を *通過済み* とメモする
   5. `now` の `portals` の各要素を `next` として見る
      1. `next` が *通過済み* ではないものなら
         1. `next` をキューに `push`

で判断できる。

問題に書いてある *入力例1* でこのアルゴリズムの動作の確認をしてみる。

> 1. `now` は `0`。*通過済み* にする。
>    1. `now` から行けるのは `1`、`2`、`3`。すべて `push`。
> 2. `now` は `1`。*通過済み* にする。
>    1. `now` から行けるのは `0`、`2`、`3`。このうち `2`、`3` を `push`。
> 3. `now` は `2`。*通過済み* にする。
>    1. `now` から行けるのは `3`。これを `push`。
> 4. `now` は `3`。*通過済み* にする。
>    1. `now` から行けるのは `4`。これを `push`。
> 5. `now` は `2`。`push` できるものはない。
> 6. `now` は `3`。`push` できるものはない。
> 7. `now` は `3`。`push` できるものはない。
> 8. `now` は `4`。到達できちゃった。

こういう手近な方法から幅広く探していくのを、**幅優先探索** (BFS、Breadth First Search) という。キューを使うことが多い。

```cpp
bool reachable = false;

std::vector<bool> passed(E); // 通過済みかどうかのメモ用

std::queue<size_t> bfs;
bfs.push(S);

while (!bfs.empty()) {
  auto now = bfs.front();
  bfs.pop();
  
  if (G == now) {
    reachable = true;
    break;
  }

  // 通過済みなので更新する必要がない場合
  if (passed[now]) {
    continue;
  }
  
  passed[now] = true;
  for (auto next : adj[now]) {
    if (!passed[next]) {
      bfs.push(next);
    }
  }
}
```

仕上げに出力を書いて終わり。

```cpp
using std::cout;
if (reachable) {
  cout << "YES\n";
} else {
  cout << "NO\n";
}
```

全体はこうなる。

```cpp
#include <iostream>
#include <queue>

int main() {
  using std::cin;
  int V, E, S, G;
  cin >> V >> E >> S >> G;

  std::vector<std::vector<size_t>> adj(V);
  for (int i = 0; i < E; ++i) {
    int src, dst;
    cin >> src >> dst;
    adj[src].push_back(dst);
  }
  
  bool reachable = false;

  std::vector<bool> passed(E); // 通過済みかどうかのメモ用

  std::queue<size_t> bfs;
  bfs.push(S);

  while (!bfs.empty()) {
    auto now = bfs.front();
    bfs.pop();

    if (G == now) {
      reachable = true;
      break;
    }

    // 通過済みなので更新する必要がない場合
    if (passed[now]) {
      continue;
    }

    passed[now] = true;
    for (auto next : adj[now]) {
      if (!passed[next]) {
        bfs.push(next);
      }
    }
  }

  using std::cout;
  if (reachable) {
    cout << "YES\n";
  } else {
    cout << "NO\n";
  }
}
```

こういった幅優先探索は、迷路やパズルを解いたり、ゲームの対戦用 CPU を作ったりといったことにまで応用できるよ。

ちなみに、*入力例2* のようにループする状況のために `passed` で記録する必要がある。逆にループしない場合は `passed` をチェック/記録する必要はない。


## priority_queue

データを入れると自動で降順 (大きい→小さい順) にソートされて、その片端からしか取り出せないコンテナ。

これは `#include <queue>` が必要。`std::queue` と同じ `queue` ファイルに入ってる。

```cpp
std::priority_queue<int> Q;

// push と pop があるけど他のとはかなり違う

Q.push(2); // {2}
Q.push(4); // {2, 4}
Q.push(3); // {2, 3, 4}
Q.push(1); // {1, 2, 3, 4}

// empty で空かどうかを bool で取得
while (!Q.empty()) {
  // top でキューの末尾を取得
  auto head = Q.top();
  // pop で末尾を削除
  Q.pop();
  std::cout << head << " ";
}
```

ソートの順番を変えるには、テンプレート引数にソートに使う関数型も渡す。

デフォルトは `std::less` になっている。これは `<` 演算子の意味があり、`前の要素 < 次の要素` になるようにソートする。

逆に `std::greater` は `>` 演算子の意味があり、`前の要素 > 次の要素` になるようにソートする。

基本的にこのどっちかを第三テンプレート引数に渡す。

何故か第三引数だから、第二引数のコンテナ型の指定もしなきゃいけない。これは `std::vector` でいい。

```cpp
std::priority_queue<
  int, // 要素型
  std::vector<int>, // std::vector<要素型> で OK
  std::greater<> // ソートに使う関数型、テンプレート引数は空でいい
> Q;

Q.push(2); // {2}
Q.push(4); // {4, 2}
Q.push(3); // {4, 3, 2}
Q.push(1); // {4, 3, 2, 1}
```


### 例題

それでは例題を (ry

> 入力:
> ```
> V E S G
> src1 dst1 duration1
> src2 dst2 duration2
> :
> srcE dstE durationE
> ```
> - `V` と　`E` は正の整数
> - `S` と `G` は `0` 以上 `E` 未満の整数
> - `srcE` と `dstE` は `0` 以上 `V` 未満の整数
> - `durationE` は正の整数
>
> `V` 個の異空間がある。
>
> 異空間どうしの間には二つの空間をつなぐ一方通行のポータルがあり、`src` 番目の空間から `dst` 番目の空間に向かってだけ移動できる。
> 
> この移動には、`duration` 秒時間がかかる。
>
> ポータルを利用すると、`S` 番目の空間から `G` 番目の空間へ必ず移動できる。
>
> この移動にかかる最小時間と、改行を出力せよ。
>
> 入力例1:
> ```
> 3 3 0 2
> 0 2 40
> 0 1 10
> 1 2 20
> ```
> 出力例1:
> ```
> 30
> ```
> ---
> 入力例2:
> ```
> 6 9 0 4
> 0 1 7
> 0 2 9
> 0 5 14
> 1 3 15
> 2 3 11
> 2 5 2
> 5 4 9
> 3 4 6
> 1 2 10
> ```
> 出力例2:
> ```
> 20
> ```

入力の処理は、`durationE` に対応するぶんを変える。

```cpp
using std::cin;
int V, E, S, G;
cin >> V >> E >> S >> G;

std::vector<std::vector<size_t>> adj(V);
std::vector<std::vector<int>> cost(V, std::vector<int>(V));
for (int i = 0; i < E; ++i) {
  int src, dst, duration;
  cin >> src >> dst >> duration;
  adj[src].push_back(dst);
  cost[src][dst] = duration; // src から dst へかかる時間をこう表現することにする
}
```

さっきの `queue` を使った幅優先探索と似たやり方で解ける。察していると思うけど、今回は `priority_queue` のほうがいい。

これを使うと、こんなアルゴリズムになる。

1. 最初の空間である `S` 番目のイテレータをキューに `push`
2. キューが空になるまで繰り返し
   1. キューの `top` を `cost`, `now` として取り出し、`pop`
   2. `now` の `portals` の各要素を `next` として見る
      1. `now` の `cost_from_S` と、`next` と同じインデックスの `portal_costs` を足す (見ている `now`)。これを `new_cost` とする
      2. `new_cost` が `next` の `cost_from_S` より小さいい場合
         1. `next` の `cost_from_S` に `new_cost` を代入
         2. `next` をキューに `push`

このアルゴリズムを、発明者にちなんでダイクストラ法っていう。

```cpp
// 最短距離のメモ用。最大値にしておいて、これから小さくしていく
std::vector<int> cost_from_S(V, 
  std::numeric_limits<int>::max() // 型の最大値を取得する関数
);
cost_from_S[S] = 0;

// pair に コストとインデックスを格納する
std::priority_queue<
  std::pair<int, size_t>,
  std::vector<std::pair<int, size_t>>,
  std::greater<>
> Q;
Q.push({0, S});

while (!Q.empty()) {
  auto pair = Q.top();
  int now_cost = pair.first;
  size_t now = pair.second;
  Q.pop();

  // cost_from_S[now] を更新する必要がない場合
  if (cost_from_S[now] < now_cost) {
    continue;
  }

  for (auto next : adj[now]) {
    auto new_cost = now_cost + cost[now][next];
    if (new_cost < cost_from_S[next]) {
      cost_from_S[next] = new_cost;
      Q.push({cost_from_S[next], next});
    }
  }
}
```

あとは出力しておわり。

```cpp
std::cout << cost_from_S[G] << "\n";
```

全体はこうなった。私にでも何日かかかるくらい難しかったから、サクサクできたならすごいよ。

```cpp
#include <iostream>
#include <queue>

int main() {
  using std::cin;
  int V, E, S, G;
  cin >> V >> E >> S >> G;

  std::vector<std::vector<size_t>> adj(V);
  std::vector<std::vector<int>> cost(V, std::vector<int>(V));
  for (int i = 0; i < E; ++i) {
    int src, dst, duration;
    cin >> src >> dst >> duration;
    adj[src].push_back(dst);
    cost[src][dst] = duration;
  }

  // 最短距離のメモ用。最大値にしておいて、これから小さくしていく
  std::vector<int> cost_from_S(V, 
    std::numeric_limits<int>::max() // 型の最大値を取得する関数
  );
  cost_from_S[S] = 0;

  // pair に コストとインデックスを格納する
  std::priority_queue<
    std::pair<int, size_t>,
    std::vector<std::pair<int, size_t>>,
    std::greater<>
  > Q;
  Q.push({0, S});

  while (!Q.empty()) {
    auto pair = Q.top();
    int now_cost = pair.first;
    size_t now = pair.second;
    Q.pop();

    // cost_from_S[now] を更新する必要がない場合
    if (cost_from_S[now] < now_cost) {
      continue;
    }

    for (auto next : adj[now]) {
      auto new_cost = now_cost + cost[now][next];
      if (new_cost < cost_from_S[next]) {
        cost_from_S[next] = new_cost;
        Q.push({cost_from_S[next], next});
      }
    }
  }

  std::cout << cost_from_S[G] << "\n";
}
```

こういうふうに経路について処理する、*グラフ理論* に関する問題を章末問題に出しておくね。
