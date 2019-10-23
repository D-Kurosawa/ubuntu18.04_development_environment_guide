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

#### <参考元>

- [pyenv](https://github.com/pyenv/pyenv/)
- [pyenv-installer](https://github.com/pyenv/pyenv-installer)
- [2018年のPythonプロジェクトのはじめかた](https://qiita.com/sl2/items/1e503952b9506a0539ea)
- [ubuntuにpyenvを導入](https://crowrabbit.hatenablog.com/entry/2019/05/14/ubuntu%E3%81%ABpyenv%E3%82%92%E5%B0%8E%E5%85%A5)
