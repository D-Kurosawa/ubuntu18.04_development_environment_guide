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

### 3-2. OpenJDK のインストール

以下のコマンドで`OpenJDK`をイントールします。今後の`OpenJDK`の主流は`AdoptOpenJDK`になるとのことなのでバージョン指定をしてインストールしても良いのですが、2019年10月時点では`SDKMAN`の標準は`AdoptOpenJDK`となっているので、そのままイントールします。

```bash
sdk install java
```

イントールが完了したら、以下のコマンドでどの`JDK`がインストールされたのか確認します。

```bash
sdk list java
```

イントールされているバージョンの`Status`が`installed`になっていることを確認してください。

```bash
java -version
```

最後に上記コマンドでバージョン情報が表示されれば成功です。

### 3-3. JAVA_HOMEの設定

通常は環境変数`JAVA_HOME`、および`PATH`はイントール時に自動設定されます。以下のコマンドで環境変数を確認してください。

```bash
echo $JAVA_HOME
echo $PATH
```

`JAVA_HOME`に何も表示されない場合は、以下のコマンドで設定し再度、環境変数を確認してください。

```bash
cd ~

# 設定の見やすくするためのヘッダなので無くても良い
echo '' >> ~/.bashrc
echo '# JAVA_HOME' >> ~/.bashrc

# JAVA_HOMEの設定
echo 'export JAVA_HOME=$HOME/.sdkman/candidates/java/current' >> ~/.bashrc
echo 'export PATH=$JAVA_HOME/bin:$PATH' >> ~/.bashrc

# .bashrcのリロード
source ~/.bashrc
```

## 4. サンプルプログラムのコンパイル＆実行

適当なディレクトリに以下のソースコードを記載します。

```bash
cd
mkdir test_compile
cd test_compile/
nano HelloWorld.java
```

```java
// [HelloWorld.java]
public class HelloWorld {
    public static void main(String[] args){
        System.out.println("Hello, World!");
    }
}
```

---

ソースを以下のコマンドでコンパイルして実行します。ターミナルに`Hello World!`と表示されれば成功です。

```bash
javac HelloWorld.java

java HelloWorld
```

## <参考元>

- [SDKMAN公式](https://sdkman.io/)
- [JDK、Oracle JDK、OpenJDK、Java SEってなに？](https://qiita.com/nowokay/items/c1de127354cd1b0ddc5e)
- [OpenJDK時代のJavaをMacで切り替える方法](https://qiita.com/seri/items/cbfe1886ec902029529d)
