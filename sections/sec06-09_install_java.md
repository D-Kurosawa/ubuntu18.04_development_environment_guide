# Java のイントール

Java(ジャバ)は、狭義ではプログラミング言語のJavaを指し、広義ではJava言語を中心にしたコンピューティング・プラットフォームを意味します。後者はJavaプラットフォームと呼ばれ、Javaの関連技術はJavaテクノロジと総称されます。Javaの構文はC++に類似したもので、オブジェクト指向を主要パラダイムとしています。Javaは、JVM(Java Virtual Machine)で動作するため、特定の環境に依存しないクロスプラットフォーム・プログラムです。

## 1. JavaSEとJavaEE

JavaSE(Java Standard Edition)を簡単に言えば**Javaの基本機能をまとめたもの**です。具体的にはJavaでプログラミングを行う際に最低限必要な機能をまとめたものです。アプリケーションを開発する場合は後述する`JDK`をインストールしておく必要があります。

JavaEE(Java Enterprise Edition)を簡単に言えば**JavaSEを元にしてサーバーサイドの機能を追加したもの**です。主にWebサイト(もしくはWebアプリケーション)を開発する際に用いられます。

|名称|機能|
|:---|:---|
|JavaSE|Javaの基本機能をまとめたもの|
|JavaEE|JavaSE + 拡張機能|

## 2. JREとJDK

JRE(Java Runtime Environment)とは**Javaで作られたアプリケーションを動かすために必要なソフト**です。Javaで書かれたプログラムを動かすために必要なプログラムとなります。

JDK(Java SE Development Kit)とは**Javaでプログラムを組む際に必要なソフト(開発キット)**です。前述したJavaSEを使ってJava言語でプログラムを書く場合にはこのJDKが必須になります。

|名称|機能|
|:---|:---|
|JRE|Javaで作られたアプリケーションを動かすために必要なソフト|
|JDK|Javaでプログラムを組む際に必要なソフト|

## 3. インストール

### 3-1. SDKMANのインストール

ubuntuでは`apt`で`Java`をインストールするこもと可能ですが、Javaをはじめとする**JVM言語**では`SDKMAN`というパッケージマネージャーを用いることでインストール、およびバージョン管理ができるので、以下のコマンドでインストールします。

```bash
curl -s "https://get.sdkman.io" | bash
```

イントール後、画面に従い以下のコマンドを入力します。

```bash
source "/$HOME/.sdkman/bin/sdkman-init.sh"
sdk help
```

ヘルプが表示されれば成功です。

## <参考元>

- [SDKMAN公式](https://sdkman.io/)
