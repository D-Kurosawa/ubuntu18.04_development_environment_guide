# Rust のイントール

**Rust**(ラスト)はMozillaが支援するオープンソースのシステムプログラミング言語です。

Rustは速度、並行性、安全性を言語仕様として保証するC言語、C++に代わるシステムプログラミングに適したプログラミング言語を目指しています。2015年に1.0版がリリースされるまでにいくつもの破壊的な仕様変更がありましたが、1.0版以降は基本的には後方互換を保って6週間間隔で定期的にリリースされています。

Rustはマルチパラダイムプログラミング言語であり、手続き型プログラミング、オブジェクト指向プログラミング、関数型プログラミングなどの実装手法をサポートしています。コンパイル基盤にMIRとLLVMを用いており、実行時速度性能はC言語と同等程度です。強力な型システムとリソース管理の仕組みにより、メモリ安全性が保証されています。

Rustは2016〜2019年の間Stack Overflow Developer Surveyで「最も愛されているプログラミング言語」で一位を獲得し続けていますが、学習難易度が高い言語とも考えられております。

## 1. インストール

Rustは、`rustup`というRustのバージョンと関連するツールを管理するコマンドラインツールを使用してダウンロードします。以下のコマンドで`rustup`をインストールします。

```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

Rustが正常にインストールされているか確かめるには、以下のコマンドを使用してください。

```bash
rustc --version
```

バージョン情報が表示されれば成功です。

## 2. サンプルプログラムのコンパイル＆実行

適当なディレクトリに以下のソースコードを記載します。

```bash
cd
mkdir test_compile
cd test_compile/
touch hello.rs
```

```rust
fn main() {
    println!("Hello, World!");
}
```

ソースを以下のコマンドでコンパイルして実行します。ターミナルに`Hello World!`と表示されれば成功です。

```bash
rustc -o hello_rust hello.rs

./hello_rust
```

### <参考元>

- [Rust本家](https://www.rust-lang.org/)
- [Rustの日本語ドキュメント](https://doc.rust-jp.rs/)
- [プログラミング言語 Rust, 2nd Edition](https://doc.rust-jp.rs/book/second-edition/)
