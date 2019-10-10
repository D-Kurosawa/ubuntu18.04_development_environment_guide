# clang のインストール

一般的にコンパイラに依存しないコードを書くには、複数のコンパイラでバイナリをはき同一の結果を得ることが最適な方法であると言われています。

C/C++コンパイラでgccと双璧をなすのが`clang`です。gccより後発でGoogleやAppleがサポートしているコンパイラです。コンパイル時のエラーメッセージが分かりやすいと評判で、実行環境にもよると推察しますがgccより高速なバイナリをはくと言われています。  \
`clang`のインストールは以下のコマンドを実行します。

```bash
sudo apt install clang
```

## 1. インストール状況の確認

clangが正しくインストールされているか否かはバージョンを表示して確認します。

```bash
clang --version
clang++ --version
```

## 2. サンプルプログラムのコンパイル＆実行

適当なディレクトリに以下の2つのソースコードを記載します。

```bash
cd
mkdir test_compile
cd test_compile/

# nanoエディタを用いてファイルの新規作成、記載
nano hello.c
nano hello.cpp
```

### サンプルコード

---

```c
// [hello.c]
#include <stdio.h>

int main(void)
{
    printf("Hello World!\n");

    return 0;
}
```

```c++
// [hello.cpp]
#include <iostream>

int main()
{
    std::cout << "Hello World!\n" << std::endl;

    return 0;
}
```

---

それぞれのソースを以下のコマンドでコンパイルして実行します。  \
ターミナルに`Hello World!`と表示されれば成功です。

```bash
# 使用したclang, clang++のバージョンは6.0.0

clang -o hello_clang6 hello.c
clang++ -o hello_clang++6 hello.cpp

./hello_clang6
./hello_clang++6
```
