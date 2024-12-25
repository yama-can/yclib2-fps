# FPS / Yama.can Library v2

## FPS とは

競技プログラミングにおける FPS であることに留意してください。

FPS とは、集合を関数として扱う、及びその逆として扱う考え方のことです。

例えば、$A=\{x_n\}$ を関数として表すと以下のようになります。

$$
A(x) = \sum^{\infty}_{i=-\infty}{A_xx^i}
$$

これを利用することで、一部の DP を高速に計算したりすることができます。

## 諸注意

計算量等で記述される $N$ は、特に記述がない限り使用される関数の $x$ の次数の総和です。

このライブラリの作成にあたり、[[多項式・形式的べき級数] 高速に計算できるものたち
](https://maspypy.com/%E5%A4%9A%E9%A0%85%E5%BC%8F%E3%83%BB%E5%BD%A2%E5%BC%8F%E7%9A%84%E3%81%B9%E3%81%8D%E7%B4%9A%E6%95%B0-%E9%AB%98%E9%80%9F%E3%81%AB%E8%A8%88%E7%AE%97%E3%81%A7%E3%81%8D%E3%82%8B%E3%82%82%E3%81%AE) を参考にしています。
積（畳み込み）については AtCoder Library（ACL）のほうが高速なため、それを使用しています。

## 一覧

| 呼称                       | 式                                                     | 計算量                    | 実装                  |
| -------------------------- | ------------------------------------------------------ | ------------------------- | --------------------- |
| 積                         | $f(x)g(x)$                                             | $O(N \log N)$             | ✅️ (ACL-Recommended) |
| 逆数                       | $1/f(x)$                                               | $O(N \log N)$             | ❌️                   |
| 対数                       | $\log f(x)$                                            | $O(N \log N)$             | ❌️                   |
| 指数                       | $\exp f(x)$                                            | $O(N \log N)$             | ❌️                   |
| べき乗                     | $(f(x))^n$                                             | $O(N \log N)$             | ❌️                   |
| 合成                       | $f(g(x))$                                              | $O(N \log^2 N)$           | ❌️                   |
| 逆関数                     | $f^{-1}(x)$                                            | $O(N \log^2 N)$           | ❌️                   |
| gcd                        | $gcd(f(x), g(x))$                                      | $O(N \log^2 N)$           | ❌️                   |
| mod                        | $f(x)\bmod g(x)$                                       | $O(N \log N)$             | ❌️                   |
| べき乗 mod                 | $(f(x))^k\bmod g(x)$                                   | $O(N \log N \log k)$      | ❌️                   |
| べき乗 mod                 | $(f(x))^k\bmod (1-x^N) (N = 2^l, l は整数)$            | $O(N \log N + N \log k)$  | ❌️                   |
| 多項式総積                 | $\prod_{i=1}^{M}f_i(x)$                                | $O(N \log^2 N)$           | ❌️                   |
| 有理式総和                 | $\sum_{i=1}^{M}\frac{f_i(x)}{g_i(x)}$                  | $O(N \log^2 N)$           | ❌️                   |
| (1-q^i) の積               | $\prod_{i=0}^{n-1}(1-q^ix)$                            | $O(N \log N)$             | ❌️                   |
| シフト                     | $f(x + c)$                                             | $O(N \log N)$             | ❌️                   |
| Stirling First             | $s(N, k)$ の $k$ に関する列挙                          | $O(N \log N)$             | ❌️                   |
| Stirling First 2 項目列挙  | $s(n, K)$ の $n$ に関する列挙                          | $O((N - K) \log (N - K))$ | ❌️                   |
| Stirling Second            | $S(N, k)$ の $k$ に関する列挙                          | $O(N \log N)$             | ❌️                   |
| Stirling Second 2 項目列挙 | $S(n, K)$ の $n$ に関する列挙                          | $O((N - K) \log (N - K))$ | ❌️                   |
| Multipoint Eval            | $\{a_n\}$ に対する $f(a_i)$ の評価                     | $O(N \log^2 N)$           | ❌️                   |
| Reversed Multipoint Eval   | $\{a_n\}$ と $f(a_i)$ からの $f(x)$ の復元             | $O(N \log^2 N)$           | ❌️                   |
| Multipoint Eval Shift      | $1\leq i \leq M$ を満たす $f(i + c)$ の評価            | $O((N+M) \log^2 (N+M))$   | ❌️                   |
| 部分分数分解               | $\{a_n\}$ に対する $x - a_i$ での部分分数分解          | $O(N \log^2 N)$           | ❌️                   |
| n^k の総和                 | $\sum_{n=0}^{N} n^k$ の評価                            | $O(k)$                    | ❌️                   |
| n^k の総和の列挙           | $1 \leq k \leq K$ における $\sum_{n=0}^{N} n^k$ の評価 | $O(K \log K)$             | ❌️                   |
| f(k) の総和                | $1 \leq k \leq K$ を満たす $k$ に対して $f(k)$ の総和  | $O(N \log N)$             | ❌️                   |
| f(kx) の総和               | $L \leq k \leq R$ を満たす $k$ に対して $f(kx)$ の総和 | $O(N \log N)$             | ❌️                   |
| f(kx) の総積               | $L \leq k \leq R$ を満たす $k$ に対して $f(kx)$ の総積 | $O(N \log N)$             | ❌️                   |

## 積

$f(x)g(x)$ を高速に求めます。
FFT の畳み込みを利用します。

計算量： $O(N \log N)$
