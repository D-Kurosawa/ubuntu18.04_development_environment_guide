# Python のインストール

Ubuntuを含むほとんどのLinuxディストリビューション(FreeBSD、NetBSD、OpenBSD、macOS)では、Pythonはデフォルトでインストールされるか、システムのパッケージリポジトリにPythonを収録しています。これらの環境の多くは、Pythonをコアコンポーネントの一部として利用しています。UbuntuのインストーラーのUbiquityなどはPython上で動作します。

このため、システムのパッケージ管理ツール(aptなど)が管理するシステムのネイティブパッケージとして、PyPI由来の数多くのパッケージをインストールできます。しかし、入手できるライブラリの数はPyPIよりも少なく、またほとんどのライブラリのバージョンが古い点に注意が必要です。

また、最新のパッケージの取得にはPyPA(Pythonパッケージオーソリティ)が推奨しているとおり`pip`コマンドを使うべきです。

## 1. 環境分離の必要性

`pip`は、システム全体で使われるパッケージのインストールに使われることが多いでしょう。UnixベースのシステムやLinuxのシステムであれば、スーパーユーザー権限が必要になるため、実際には以下の形式で実行されます。

```bash
sudo pip install <パッケージ名>
```

システム全体から使われるパッケージをPyPIから直接インストールすることはお勧めできませんし、避けるべきです。これは先に説明したように、Pythonはオペレーティングシステムのパッケージリポジトリ経由でインストールされる多くのパッケージの一部として使われる場合が非常に多く、重要なシステムを駆動するのにも使われているからです。システムディストリビューションのメンテナーは、さまざまなパッケージの依存関係が壊れないように、パッケージのバージョンの選択に非常に多くの時間を費やしています。そのために、システムのリポジトリで提供されるPythonのパッケージは、システム固有のパッチが当たっていたり、他のコンポーネントとの依存関係を維持するために、古いバージョンに固定されていることがあります。これらのパッケージを`pip`を使って強制的に最新バージョンに更新してしまうと、後方互換性が崩れたり、システムの重要なサービスが動作しなくなることがあります。

開発目的だとしてもローカルコンピュータ上でこのようなことをやって良いことにはなりません。`pip`を強引に使用すると、デバッグが非常に難しいトラブルに遭遇する確率が上がります。`pip`を使ってシステムグローバルにパッケージをインストールすることを禁止はしませんが、リスクを認識しながら行うべきことであると意識してください。

この問題は、環境を分離することで簡単に解決できます。環境分離のためのツールは数種類あり、それぞれ異なるレベルでシステムを抽象化し、Pythonランタイム環境を分離します。もっともよく利用されるのが、システム、サービス、プロジェクト、それぞれに必要なパッケージや依存関係を分離するツールです。この方法には次のようなメリットがあります。

- プロジェクトXはライブラリZのバージョン1.xに依存するが、プロジェクトYはライブラリZの4.x を必要とする、というジレンマを解消します。依存ライブラリが衝突してお互いに影響を与えるようなリスクを避けながら、開発者が複数のプロジェクトを同時に扱えるようになります。
- 開発者が、使用しているシステムのディストリビューションリポジトリで提供されているパッケージのバージョンに拘束されることなく、プロジェクト側で任意に決定できるようになります。
- 開発環境にのみ導入された新しいパッケージのバージョンにより、特定のパッケージバージョンに依存するシステムサービスに影響を与えることがなくなります。
- プロジェクトに依存関係のあるパッケージ一覧を簡単に**凍結**できます。新しいシステム上で
同じ環境を再現することが簡単に行えます。

環境を分離する方法のうち、もっとも簡単で軽量な方法が、アプリケーションレベルの仮想環境を使用することです。この方法は、Pythonインタープリタとそれが使用可能なパッケージを分離することだけを行います。セットアップは簡単で、小さいプロジェクトやパッケージの開発には十分です。

## 2. pyenv のインストール

### 2-1. 前提条件

独自のPythonインタープリターをコンパイルするには、適切なライブラリとパッケージのインストールが必要です。以下のコマンドで必要なパッケージのインストールします。

```bash
sudo apt update
sudo apt -y upgrade
sudo apt -y install make build-essential libssl-dev zlib1g-dev libbz2-dev \
libreadline-dev libsqlite3-dev wget curl llvm libncurses5-dev libncursesw5-dev \
xz-utils tk-dev libffi-dev liblzma-dev python-openssl git
```

### 2-2. インストール

今回は[pyenv本家](https://github.com/pyenv/pyenv/)からではなく、[インストーラー](https://github.com/pyenv/pyenv-installer)を用いてインストールします。

以下のコマンドにて`curl`でinstallerをダウンロードして実行します。

```bash
curl -L https://github.com/pyenv/pyenv-installer/raw/master/bin/pyenv-installer | bash
```

インストールが進むと以下のようなメッセージが表示されます。

```bash
WARNING: seems you still have not added 'pyenv' to the load path.

# Load pyenv automatically by adding
# the following to ~/.bashrc:

export PATH="$HOME/.pyenv/bin:$PATH"
eval "$(pyenv init -)"
eval "$(pyenv virtualenv-init -)"
```

これは`pyenv`へのパスが通ってない警告で、コメントアウト以降を使用しているシェルの設定ファイルに記載することで自動的にパスが通ります。

以下のコマンドでパスを通します。

```bash
# 設定の見やすくするためのヘッダなので無くても良い
echo '' >> ~/.bashrc
echo '# pyenv' >> ~/.bashrc

# pyenvのパスを通す
echo 'export PATH="$HOME/.pyenv/bin:$PATH"' >> ~/.bashrc
echo 'eval "$(pyenv init -)"' >> ~/.bashrc
echo 'eval "$(pyenv virtualenv-init -)"' >> ~/.bashrc

# .bashrcのリロード
source ~/.bashrc

pyenv --version
```

バージョン情報が表示されたらインストールが成功です。

### 2-2. アップデート

`pyenv`自体は以下のコマンドでアップデートします。

```bash
pyenv update
```

### 2-3. pyenvでPythonのインストール

以下のコマンドでイントール可能な`Python`のバージョンを確認します。

```bash
pyenv install --list
```

指定したバージョンのインストールは以下のコマンドで行います。

```bash
pyenv install [バージョン名]

# Python 3.7.5をインストールする場合は
pyenv install 3.7.5
```

インストール済みの`Python`を以下のコマンドで確認します。

```bash
pyenv versions
* system (set by /home/hogehoge/.pyenv/version)
  2.7.17
  3.5.7
  3.6.9
  3.7.5
  3.8.0
```

先頭に`*`がついているバージョンが現在グローバルに設定されているものを示します。  \
グローバルバージョンの切替は以下のコマンドで行います。

```bash
pyenv global 3.7.5

# バージョンが切り替わっているか確認
pyenv versions
  system
  2.7.17
  3.5.7
  3.6.9
* 3.7.5 (set by /home/hogehoge/.pyenv/version)
  3.8.0

# グローバルの再確認
python --version
```

先頭に`*`がついているバージョンとグローバルが一致していれば成功です。

これで任意のバージョンの`Python`をいつでも使えるようになりました。

## 3. pipenv のインストール

### 3-1. pipの更新

`pipenv`は`pip`を用いてインストールするため、まずは`pip`を更新します。

ただし、システム自身の`pip`は更新したくないため、`pyenv`でグローバルを`system`以外のバージョンに設定したのちに以下のコマンドで`pip`を更新します。

```bash
pip install --upgrade pip
```

### 3-2. インストール

以下のコマンドで`pipenv`をインストールします。

```bash
pip install --user pipenv
```

ここでパスが通っていないと警告が出た場合はターミナルを再起動します。通常は`$HOME/.local/bin`に対して自動でパスが通ります。

```bash
pipenv --version
```

バージョン情報が表示されたらインストールが成功です。

この状態でもパスが通っていなければ、以下のコマンドでパスを通します。

```bash
# 設定の見やすくするためのヘッダなので無くても良い
echo '' >> ~/.bashrc
echo '# ~/.local/bin (pipenv)' >> ~/.bashrc

# ~/.local/binのパスを通す
echo 'export PATH="$PATH:$HOME/.local/bin"' >> ~/.bashrc

# .bashrcのリロード
source ~/.bashrc

pipenv --version
```

#### 通常のインストールとユーザーインストールの違い

- [pip公式 doc : User Installs](https://pip.pypa.io/en/stable/user_guide/#user-installs)

`pip`でパッケージをインストールする場合、`pip install`コマンドに`--user`オプションを付けるか否かの2通りの方法があります。`--user`オプションを付けるインストール方法をユーザーインストールと言います。それぞれの方法でインストールする場合以下のようになります。

|インストール方法|コマンド|管理者権限|イントール先|
|:---|:---|:---|:---|
|通常|`pip install [package名]`|必要|`/usr`下|
|ユーザーインストール|`pip install --user [package名]`|必要ない|`~/.local`下|

これらの違いはパッケージがインストールされるディレクトリです。通常のインストールでは管理者権限が必要な`/usr`下に、ユーザーインストールでは管理者権限の必要ないホームディレクトリ下の`~/.local`(ベースバイナリディレクトリという)下にパッケージがインストールされます。

#### ユーザーインストールのメリット

- 管理者権限のないユーザが`pip`でパッケージ管理を行えます。
- システム全体に関わるパッケージを壊さずにパッケージをインストールできます(システムを汚さない)。

#### ユーザーインストール先の確認

ユーザーインストールでパッケージをインストールした場合、デフォルトとしてベースバイナリディレクトリ`~/.local`下にパッケージがインストールされます。  \
以下のコマンドでベースバイナリディレクトリを出力することができます。

```bash
python -m site --user-base
```

#### ユーザーインストール先の変更

ベースバイナリディレクトリは`PYTHONUSERBASE`という環境変数を設定することで変更できます。例えば、`~/.local`ではなく`~/my-python`というディレクトリに変更したい場合は、以下のコマンドで`.bashrc`を変更することで設定できます。

```bash
echo 'export PYTHONUSERBASE=~/my-appenv' >> ~/.bashrc
```

#### ユーザーインストールしたパッケージの更新

以下のコマンドでユーザーインストールしたパッケージを更新できます。

```bash
pip install --user --upgrade [packege名]
```

## 4. pipenv の仮想環境操作

### 4-1. プロジェクトの初期化

新規のプロジェクトの初期化方法は、プロジェクトのディレクトリに移動して以下のコマンドを実行します。

```bash
# Python3系で初期化する例
pipenv --python 3
```

自動で仮想環境が作成されて`Pipfile`というファイルが生成されます。

Pythonのバージョンの指定は3.7などより詳細に指定もできます。また`pyenv`を使用しているので、環境に入っていないバージョンを指定したときには`pyenv`と連動してPythonのインストールが自動的に行われます。

```bash
# Python3.8.0で初期化する例
pipenv --python 3.8.0
```

### 4-2. プロジェクトのディレクトリに仮想環境を作成

`pipenv`で仮想環境を作成すると通常は`$HOME/.local/share/virtualenvs`下に仮想環境が作成されます。仮想環境が多くなってくると、どのプロジェクトがどの仮想環境を使用しているのかの判断が難しくなってくるので、プロジェクト直下のディレクトリに仮想環境を作成するように変更します。

以下のコマンドでプロジェクトのディレクトリに仮想環境を作成するよう変更できます。

```bash
# 設定の見やすくするためのヘッダなので無くても良い
echo '' >> ~/.bashrc
echo '# pipenv directory' >> ~/.bashrc

# 環境変数を設定
echo 'export PIPENV_VENV_IN_PROJECT=true' >> ~/.bashrc
```

この設定後にプロジェクトの初期化を行うと`./.venv`ディレクトリに仮想環境が生成されます。

### 4-3. 仮想環境の削除

以下のコマンドで仮想環境を削除できます。

```bash
pipenv --rm
```

### 4-4. 仮想環境の確認

以下のコマンドで仮想環境のパスを確認できます。

```bash
pipenv --venv
```

### 4-5. パッケージのインストール

以下のコマンドでパッケージのインストールを行います。

```bash
# Pandasをインストールする例
pipenv install pandas
```

まだ仮想環境を作っていなければ仮想環境が自動的に作られて、そこにパッケージがインストールされます。また`pipenv`からパッケージをインストールすると`Pipfile`にパッケージが追加されます。このときに`Pipfile.lock`が自動で生成され、実際にインストールされたパッケージの詳細なバージョンや依存パッケージの情報などが記録されます。これをもとに他PCで環境を再現することが簡単にできます。

`pipenv`を使うとパッケージの管理や仮想環境の生成が自動的に行われて便利です。  \
チームにPythonに不慣れなメンバーがいても、pipを直接使ってrequirements.txtを更新し忘れてしまったり、仮想環境を作らずに関係のないパッケージまで管理されてしまったりを防ぐことができます。

また、バージョンを指定してパッケージをインストールする場合は以下のコマンドのように行います。

```bash
# Pandas 0.24.2 をインストールする例
pipenv install pandas==0.24.2
```

### 4-6. 仮想環境の出入

仮想環境へは以下のコマンドで入ります。

```bash
pipenv shell
```

また、仮想環境を出る際は`exit`だけでOKです。

スクリプトは`Profile`への登録と`pipenv run`を組み合わせることでdebugなどを行うことができますが、スクリプトに登録するほどでもないPythonのコードを個別に実行する際は、以下のコマンドを使用することで仮想環境に入らずにPythonのコードを実行できます。

```bash
# main.pyを実行する例
pipenv run python main.py
```

### 4-7. サンプルプログラム実行

適当なディレクトリに以下のソースコードを記載します。

```bash
cd
mkdir test_compile/hello_py
cd test_compile/hello_py/
nano hello.py
```

---

```python
# [hello.py]
import platform


def main():
    print('Hello, World!', 'version', platform.python_version())


if __name__ == '__main__':
    main()
```

ここでスクリプトを実行します。

```bash
python hello.py
```

`Hello, World!`の後ろに`pyenv`で設定したグローバルバージョンが表示されれば成功です。

---

次に仮想環境を作成します。ここでは`3.8.0`を指定しています。グローバルバージョンと異なるバージョンを指定しないとテストが成功したか分からないので、その点に注意してください。

```bash
pipenv --python 3.8.0
```

仮想環境を使用して作成したスクリプトを実行します。

```bash
pipenv run python hello.py
```

`Hello, World! version 3.8.0`と表示されれば成功です。

## <参考元>

- [pyenv](https://github.com/pyenv/pyenv/)
- [pyenv-installer](https://github.com/pyenv/pyenv-installer)
- [2018年のPythonプロジェクトのはじめかた](https://qiita.com/sl2/items/1e503952b9506a0539ea)
- [Ubuntu18.04にpyenvをインストールして美しいpython環境を構築する](https://www.komee.org/entry/2018/10/25/120000)
- [Ubuntuにpyenvを導入](https://crowrabbit.hatenablog.com/entry/2019/05/14/ubuntu%E3%81%ABpyenv%E3%82%92%E5%B0%8E%E5%85%A5)
- [Pipenvと仮想環境](https://pipenv-ja.readthedocs.io/ja/translate-ja/install.html#installing-pipenv)
- [Python：pip における管理者権限と user install](https://pyteyon.hatenablog.com/entry/2019/05/24/003924)
- [Pipenvを使ったPython開発まとめ](https://qiita.com/y-tsutsu/items/54c10e0b2c6b565c887a)
