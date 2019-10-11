# Fortran のインストール

数値計算にFortranを使用する機会が多いのでFortran環境を構築します。

## 1. Fortran コンパイラのインストール

### 1-1. gfortran のインストール

Fortranのコンパイラである`gfortran`をインストールするには、以下のコマンドを実行します。

```bash
sudo apt install gfortran
```

`gfortran`が正しくインストールされているか否かはバージョンを表示して確認します。  \
執筆時点でのバージョンは`7.4.0`でした。

```bash
gfortran --version
```

### 1-2. 最新バージョンのインストール

`PPA`を登録してあるので最新版（執筆時点では9.x.x）の`gfortran`をインストールすることができます。  \
以下のコマンドで複数バージョンをインストールします。

```bash
sudo apt install gfortran-8 gfortran-9
```

```bash
gfortran-8 --version
gfortran-9 --version
```

### 1-3. バージョンの切替

以下のコマンドで`gfortran`の`alternatives`を登録して`--config`オプションで設定します。

```bash
sudo update-alternatives --install /usr/bin/gfortran gfortran /usr/bin/gfortran-7 70
sudo update-alternatives --install /usr/bin/gfortran gfortran /usr/bin/gfortran-8 80
sudo update-alternatives --install /usr/bin/gfortran gfortran /usr/bin/gfortran-9 90
```

```bash
sudo update-alternatives --config gfortran
```

```bash
alternative gfortran (/usr/bin/gfortran を提供) には 3 個の選択肢があります。

  選択肢    パス               優先度  状態
------------------------------------------------------------
* 0            /usr/bin/gfortran-9   90        自動モード
  1            /usr/bin/gfortran-7   70        手動モード
  2            /usr/bin/gfortran-8   80        手動モード
  3            /usr/bin/gfortran-9   90        手動モード

現在の選択 [*] を保持するには <Enter>、さもなければ選択肢の番号のキーを押してください:
```

### 1-4. サンプルプログラムのコンパイル＆実行

適当なディレクトリに以下のソースコードを記載します。

```bash
cd
mkdir test_compile
cd test_compile/
nano hello.f90
```

```fortran
! [hello.f90]
program hello
    implicit none

    print *, "Hello, World!"
end program hello
```

ソースを以下のコマンドでコンパイルして実行します。ターミナルに`Hello World!`と表示されれば成功です。

```bash
# 使用したgfortranのバージョンは8.3.0

gfortran hello.f90 -o hello_gfortran8

./hello_gfortran8
```
