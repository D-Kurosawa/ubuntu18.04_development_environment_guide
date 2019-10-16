# Fortran のインストール

## 1. Fortran コンパイラのインストール

数値計算に`Fortran`を使用する機会が多いので`Fortranコンパイラ`をインストールし環境構築します。

### 1-1. gfortran のインストール

Fortranコンパイラである`gfortran`をインストールするには、以下のコマンドを実行します。

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

---

```fortran
! [hello.f90]
program hello
    implicit none

    print *, "Hello, World!"
end program hello
```

---

ソースを以下のコマンドでコンパイルして実行します。ターミナルに`Hello World!`と表示されれば成功です。

```bash
# 使用したgfortranのバージョンは8.3.0

gfortran hello.f90 -o hello_gfortran8

./hello_gfortran8
```

## 2. MPI のインストール

Fortranの数値計算では計算を効率的に行うために`MPI`を使用する機会が多いのでインストールします。

### 2-1. OpenMPI のインストール

今回は使用している人数が最も多いとされている`OpenMPI`を以下のコマンドでインストールします。

```bash
# libopenmpi-devをインストールしないとコンパイルできません
sudo apt install openmpi-bin libopenmpi-dev
```

```bash
# バージョン確認（３つ方法があります）
ompi_info
mpiexec --version
mpirun --version
```

### 2-2. サンプルプログラムのコンパイル＆実行

適当なディレクトリに以下のソースコードを記載します。

```bash
cd
mkdir test_compile
cd test_compile/
nano mpi_hello.f90
```

---

```fortran
! [mpi_hello.f90]
program mpi_hello
    implicit none
    include 'mpif.h'

    integer :: mpi_err = 1
    integer :: mpi_size = 1
    integer :: my_rank = 0

    call mpi_init(mpi_err)

    if (mpi_err == 0) then
        call mpi_comm_size(MPI_COMM_WORLD, mpi_size, mpi_err)
        call mpi_comm_rank(MPI_COMM_WORLD, my_rank, mpi_err)
    end if

    print *, 'size:', mpi_size, 'rank:', my_rank, 'Hello, World!'

    call mpi_finalize(mpi_err)
end program mpi_hello
```

ここで重要なのは`include 'mpif.h'`です。MPIヘッダファイルをincludeすることでMPIが使用可能になります。

---

以下のコマンドでコンパイルして実行します。MPI用のコンパイラは`mpif90`です。  \
ただし、`mpif90 --version`としても裏でコンパイルしているコンパイラのバージョンが表示されます・

```bash
mpif90 -o mpi_hello_openmpi mpi_hello.f90
```

---

MPIは`mpiexec`、もしくは`mpirun`を用いて実行します。  \
ターミナルに`Hello World!`と2回表示されれば成功です。

```bash
# mpiexecを用いた実行
mpiexec -np 2 ./mpi_hello_openmpi

# mpiexecを用いた実行
mpirun -np 2 ./mpi_hello_openmpi
```
