# エディタのインストール

## 1. Visual Stadio Code のインストール

Ubuntuのエディタといったら「VimかEmacsでしょ」と言われそうですが、普段から使い慣れている`Visual Stadio Code`をインストールします。Vimは追って。。。

1. `Chrome`で`vs code`と検索し[公式サイト](https://code.visualstudio.com/)にいく
1. `Download`をクリックする
1. `Linux`の下の方にある`Snap Store`を選択
1. `Install`をクリック
1. `Install using the command line`に記載されているコマンドをターミナルに入力
1. ターミナルに`code`と入力して`Visual Stadio Code`が立ち上げればOK

簡単ですね。これからは**Snapを用いたアプリ導入**が主流になるみたいなので使ってみました。

## 2. nano のインストール

ターミナルで直接処理できるエディタは便利なので、コンソールエディタのなかで「最も優しい」エディタといわれる`nano`をインストールします。

```bash
sudo apt install nano
```

ターミナルに`nano`と入力し`nano`が立ち上げればOK。`Ctrl + x`を入力し`nano`を終了します。

## 3. vim のインストール

Linuxの標準的なエディタなので使う使わないは別として`vim`もインストールしておきます。

```bash
sudo apt install vim
```

ターミナルに`vim`と入力し`vim`が立ち上げればOK。`:q`を入力し`vim`を終了します。

なお、ターミナルに`vimtutor`と入力すると`vim`のチュートリアルが立ち上がるので、1度やってみることをおすすめします。
