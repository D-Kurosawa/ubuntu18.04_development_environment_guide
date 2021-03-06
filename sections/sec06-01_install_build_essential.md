# ビルドツールのインストール

Linuxでアプリケーションをインストールする際は、ソースコードをダウンロードし自らビルドすることが往々にしてあります。そのため、C/C++等のビルドツールである`build-essential`をインストールする必要があります。  \
`build-essential`のインストールは以下のコマンドを実行します。

```bash
sudo apt install build-essential

# 不要なパッケージが検出された場合は以下のコマンドを実行
sudo apt autoremove
```

## 1. インストール状況の確認

gcc, g++, makeが正しくインストールされているか否かはバージョンを表示して確認します。

```bash
gcc --version
g++ --version
make --version
```

上記の3つは、他のアプリをビルドして使用する際に良く使用しますし、アプリ内のシェルスクリプトにビルドが記載されていることもありますので必ず入れておきましょう。

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
# 使用したgcc, g++のバージョンは7.4.0

gcc -o hello_gcc7 hello.c
g++ -o hello_g++7 hello.cpp

./hello_gcc7
./hello_g++7
```
