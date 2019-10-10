# 複数バージョンのインストール

コンパイラの新しいバージョンには、新しい言語のサポート、パフォーマンスの向上、拡張機能が含まれています。通常インストールしたコンパイラとは別のバージョンをインストールして複数バージョンのコンパイラを使い分けることができます。

## 1. gccの別バージョンインストール

執筆時点のUbuntu標準のgccのバージョンは`7.x.x`で、Ubuntu公式リポジトリには`v5.x.x`～`8.x.x`までのいくつかのバージョンが登録されています。  \
公式リポジトリには登録されてませんが、gccの最新バージョンは`9.2.0`です。

### 1-1. デフォルトリポジトリからのインストール

gcc 8.x.xは公式リポジトリに登録されているので、以下のコマンドでインストールできます。

```bash
sudo apt install gcc-8 g++-8
```

```bash
gcc-8 --version
g++-8 --version
```

### 1-2. PPAからのインストール

PPAとはUbuntuユーザーのチームや個人がそれぞれ管理している非公式リポジトリで、Ubuntuの公式レポジトリからはダンロードできないソフトウェアや最新のバージョンのソフトウェアを手に入れることができます。より多くのソフトウェアを試すことができる反面、動作保証や安全性の保証はないので一定のリスクがあることを確認して下さい。

PPAの便利なところは、一度、信頼するソフトウェアの提供元として追加すると、公式レポジトリからダウンロードしたソフトウェアと同様にアップデートがあるかどうか自動でチェックし、一括アップデートできることです。ソフトウェアのインストールや削除も公式のソフトと同じように、Ubuntuのソフトウェアセンターから一元管理できます。

gccの最新バージョンは[Toolchain test builds](https://launchpad.net/~ubuntu-toolchain-r/+archive/ubuntu/test)からインストールします。

### 1-3. リポジトリの登録

`add-apt-repository`コマンドを`software-properties-common`で追加インストールした後に`ubuntu-toolchain-r/test`PPAリポジトリをシステムに登録します。

```bash
sudo apt install software-properties-common
sudo add-apt-repository ppa:ubuntu-toolchain-r/test
```

### 1-4. 最新版のインストール

以下のコマンドでgccの最新版をインストールします。

```bash
sudo apt install gcc-9 g++-9
```

```bash
gcc-9 --version
g++-9 --version
```

### 1-4. バージョンの切替

バージョンの切替には`update-alternatives`コマンドを使用します。このコマンドを使用すると任意の環境に切替えることができます。

以下のコマンドで`alternatives`として登録があれば`--config`オプションで設定します。

```bash
sudo update-alternatives --list gcc
```

無い場合は以下のコマンドで登録します。  \
コマンドは、各バージョンの代替を構成し、優先度をそれに関連付けます。デフォルトのバージョンは、最も高い優先度を持つもので、この場合はgcc-9です。

```bash
sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-9 90 --slave /usr/bin/g++ g++ /usr/bin/g++-9 --slave /usr/bin/gcov gcov /usr/bin/gcov-9
sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-8 80 --slave /usr/bin/g++ g++ /usr/bin/g++-8 --slave /usr/bin/gcov gcov /usr/bin/gcov-8
sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-7 70 --slave /usr/bin/g++ g++ /usr/bin/g++-7 --slave /usr/bin/gcov gcov /usr/bin/gcov-7
```

登録がある場合は、以下のコマンドで表示されます。

```bash
sudo update-alternatives --list gcc
```

```bash
/usr/bin/gcc-7
/usr/bin/gcc-8
/usr/bin/gcc-9
```

次に通常使用するバージョンを以下コマンドで切替えます。

```bash
sudo update-alternatives --config gcc
```

```bash
alternative gcc (/usr/bin/gcc を提供) には 3 個の選択肢があります。

  選択肢    パス          優先度  状態
------------------------------------------------------------
* 0            /usr/bin/gcc-9   90        自動モード
  1            /usr/bin/gcc-7   70        手動モード
  2            /usr/bin/gcc-8   80        手動モード
  3            /usr/bin/gcc-9   90        手動モード

現在の選択 [*] を保持するには <Enter>、さもなければ選択肢の番号のキーを押してください:
```

今回は`gcc-8`を選択したので`gcc --version`で8.3.0と表示されました。

## 2. clangの別バージョンインストール

執筆時点では公式リポジトリに'8.x.x`までのバージョンがいくつかの登録されています。

### 2-1. 最新バージョンのインストール

今回は`apt`コマンドを使用して公式リポジトリの最新版をインストールします。

```bash
sudo apt install clang-7
sudo apt install clang-8
```

### 2-2. バージョンの切替

以下のコマンドで`clang`、および`clang++`の`alternatives`を登録して`--config`オプションで設定します。

---

```bash
sudo update-alternatives --install /usr/bin/clang clang /usr/bin/clang-6.0 60
sudo update-alternatives --install /usr/bin/clang clang /usr/bin/clang-7 70
sudo update-alternatives --install /usr/bin/clang clang /usr/bin/clang-8 80
```

```bash
sudo update-alternatives --config clang
```

```bash
alternative clang (/usr/bin/clang を提供) には 3 個の選択肢があります。

  選択肢    パス                優先度  状態
------------------------------------------------------------
* 0            /usr/bin/clang-8     80        自動モード
  1            /usr/bin/clang-6.0   60        手動モード
  2            /usr/bin/clang-7     70        手動モード
  3            /usr/bin/clang-8     80        手動モード

現在の選択 [*] を保持するには <Enter>、さもなければ選択肢の番号のキーを押してください:
```

---

```bash
sudo update-alternatives --install /usr/bin/clang++ clang++ /usr/bin/clang++-6.0 60
sudo update-alternatives --install /usr/bin/clang++ clang++ /usr/bin/clang++-7 70
sudo update-alternatives --install /usr/bin/clang++ clang++ /usr/bin/clang++-8 80
```

```bash
sudo update-alternatives --config clang++
```

```bash
alternative clang++ (/usr/bin/clang++ を提供) には 3 個の選択肢があります。

  選択肢    パス                優先度  状態
------------------------------------------------------------
* 0            /usr/bin/clang++-8     80        自動モード
  1            /usr/bin/clang++-6.0   60        手動モード
  2            /usr/bin/clang++-7     70        手動モード
  3            /usr/bin/clang++-8     80        手動モード

現在の選択 [*] を保持するには <Enter>、さもなければ選択肢の番号のキーを押してください:
```
