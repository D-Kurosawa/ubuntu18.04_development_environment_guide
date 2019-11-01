# Kotlin のイントール

Kotlin(コトリン)はプログラムをコンパイルし**JVM**上で動作させる事ができる静的型付けオブジェクト指向言語です。統合開発環境である`IntelliJ IDEA`で有名なJetBrainsが主導して開発したプログラミング言語で、2017年からはAndroid開発への公式言語として正式に採用されています。

KotlinはJavaと相互運用できるような設定で作られており、Kotlinを使用してJavaコードを呼び出す事ができ、その逆もまた然りです。更にJavaよりも簡潔で安全に記述する事が可能です。

## 1. イントール

KotlinのイントールにはJVM言語のパッケージマネージャーである**SDKMAN**を使用します。以下のコマンドでKotlinをインストールします。

```bash
sdk install kotlin
```

```bash
kotlin -version
```

上記のコマンドでバージョン情報が表示されれば成功です。

## サンプルプログラムのコンパイル＆実行

適当なディレクトリに以下のソースコードを記載します。

```bash
cd
mkdir test_compile
cd test_compile/
nano hello.kt
```

```kotlin
// [hello.kt]
fun main(args: Array<String>) {
   println("Hello, World!")
}
```

---

ソースを以下のコマンドでコンパイルして実行します。ターミナルに`Hello World!`と表示されれば成功です。

```bash
kotlinc hello.kt

# ここで<HelloKt.class>というファイルが作成されるので実行
# 実行にはJavaと同様に拡張子(.class)は要らない

kotlin HelloKt
```

また、`-include-runtime`オプションを指定すると、Kotlinのランタイムも一緒にまとめた`jar`ファイルが生成されます。よって、普通に**java**コマンドで実行できますので、Kotlinをインストールしていない環境でも実行できます。

```bash
kotlinc hello.kt -include-runtime -d hellokt.jar

java -jar hellokt.jar
```

## <参考元>

- [Kotlin公式](https://dogwood008.github.io/kotlin-web-site-ja/)
- [あああ](https://www.utakata.work/entry/20180124/1516781378)
- [いいい](https://qiita.com/hiko1129/items/5ac2ffe82e83bb53f08f)
