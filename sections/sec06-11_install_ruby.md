# Ruby のイントール

Ruby(ルビー)は、まつもとゆきひろ氏により開発されたオブジェクト指向スクリプト言語です。日本で開発されたプログラミング言語としては初めて国際電気標準会議(IEC)で国際規格に認証されたました。

Rubyはシンプルで書きやすく、オブジェクト指向が手軽に学べるという点が特徴的です。海外では下火になりつつありますが、**Ruby on Rails**というWebアプリケーションフレームワークで組まれたシステムが既にたくさん存在しているため、現在でも需要の高い言語となっています。

## 1. イントール

### 1-1. rbenv のインストール

rbenvはRubyのバージョンを管理するパッケージマネージャーです。[rbenv公式](https://github.com/rbenv/rbenv)のインストールガイドに従って、以下のコマンドでインストールします。

```bash
git clone https://github.com/rbenv/rbenv.git ~/.rbenv
```

```bash
# 設定の見やすくするためのヘッダなので無くても良い
echo '' >> ~/.bashrc
echo '# rbenv' >> ~/.bashrc

# rbenvの設定
echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc
echo 'eval "$(rbenv init -)"' >> ~/.bashrc

# .bashrcのリロード
source ~/.bashrc
```

以下のコマンドでバージョン情報が表示されれば成功です。

```bash
rbenv --version
```

また、以下のruby-doctorスクリプトを利用すると、rbenvの設定状態が確認できます。

```bash
curl -fsSL https://github.com/rbenv/rbenv-installer/raw/master/bin/rbenv-doctor | bash
```

### 1-2. ruby-build のインストール

rbenvでRubyをイントールするには`ruby-build`のビルドシステムが必要です。`ruby-build`には必要となる`apt`パッケージがありますので[ruby-buildのWiki](https://github.com/rbenv/ruby-build/wiki)を参考に、以下のコマンドでインストールします。

```bash
sudo apt install autoconf bison build-essential libssl-dev libyaml-dev libreadline6-dev \
zlib1g-dev libncurses5-dev libffi-dev libgdbm5 libgdbm-dev
```

次に[ruby-build公式](https://github.com/rbenv/ruby-build)に従って、以下のコマンドでインストールします。

```bash
git clone https://github.com/rbenv/ruby-build.git ~/.rbenv/plugins/ruby-build
```

ruby-doctorスクリプトで**rbenv install**の項目を確認して`ruby-build`のパスが表示されれば成功です。

```bash
curl -fsSL https://github.com/rbenv/rbenv-installer/raw/master/bin/rbenv-doctor | bash
```

### 1-3. Rubyのインストール

以下のコマンドを用いてインストールできる`Ruby`のバージョンを確認します。

```bash
rbenv install --list
```

2019年10月時点での最新安定版は`2.6.5`なので選択してインストールします。

```bash
rbenv install 2.6.5
```

次に`rbenv`の`global`にイントールしたバージョンを設定します。

```bash
rbenv global 2.6.5
```

最後に以下のコマンドでバージョン情報が表示されれば成功です。

```bash
ruby --version
```

## <参考元>

- [Ruby公式](https://www.ruby-lang.org/ja/)
- [rbenv公式](https://github.com/rbenv/rbenv)
- [ruby-build公式](https://github.com/rbenv/ruby-build)
- [Ubuntuでのrbenv,ruby-buildの使い方と事前準備に必要なaptパッケージ](https://qiita.com/tatsurou313/items/2a67075ae2416922bff0)
