# Go のインストール

**Go**はGoogleが開発したオープンソースのプログラミング言語です。よく「Golang」や「Go言語」という呼称が使用されていますが、正式名称は「**Go**」です。

特徴はGoogleによると・・・

- シンプルな言語である
- コンパイル・実行速度が早い
- 安全性が高い
- 同期処理が容易に行える
- なにより楽しい
- オープンソースである

2019年時点ではトレンドの言語なので開発環境を構築します。

## 1. イントール

Goのインストールには様々な方法がありますが、今回は[Go公式Wiki](https://github.com/golang/go/wiki/Ubuntu)を参考にインストールします。

`snap`でもインストール可能ですが、`apt`でのインストールと重複すると**PATH環境変数**では`/usr/local/bin`の方が`/snap/bin`より上位になるため環境変数の更新が必要となります。

`apt`では**Go**の最新版が取得できないため、ここでは`PPA`を設定しています。

```bash
sudo add-apt-repository ppa:longsleep/golang-backports
sudo apt install golang-go

go version
```

バージョン情報が設定されれば成功です。

### 2. サンプルプログラムのコンパイル＆実行

適当なディレクトリに以下のソースコードを記載します。

```bash
cd
mkdir test_compile
cd test_compile/
nano hello.go
```

```go
// [hello.go]
package main

import "fmt"

func main() {
  fmt.Println("Hello, World!")
}
```

---

ソースを以下のコマンドでコンパイルして実行します。ターミナルに`Hello World!`と表示されれば成功です。

```bash
go build -o hello_go hello.go

./hello_go
```

コンパイルして実行するのがダルいという場合は`go run`を用いると、自動的にコンパイル・実行してくれますが、カレントディレクトリ以下の全ファイルを読み込むわけではないという点には注意が必要です。

```bash
go run hello.go
```

### 3. 環境変数 GOPATH

Goでは、環境変数GOPATHに保存されたパス(以降、単にGOPATHと呼びます)を開発時の作業ディレクトリとして扱います。 GOPATHを利用することで、外部ライブラリの導入やビルド作業を非常に簡単に行うことができます。Go1.8よりGOPATHが未設定の場合、デフォルトで`$HOME/go`となるようになりました。

#### 3-1. GOPATHの構成

GOPATH直下には、3つのディレクトリを配置します。

- bin
  - 実行ファイルが格納されるディレクトリ(go installコマンドで自動生成されます)
- pkg
  - ビルドしたパッケージオブジェクトが格納されるディレクトリ(go buildをはじめとする各種コマンドで自動生成されます)
- src
  - パッケージごとのソースコードを配置するディレクトリ。srcディレクトリは最初に自分で用意する必要がありますが、他のディレクトリはコマンドで自動生成されます。

#### 3-2. GOPATHの確認

以下コマンドでGoの環境を確認できます。`GOPATH`がどこに設定されているか確認をしてください。

```bash
go env
```

#### 3-3. GOPATHの設定

以下のコマンドで環境変数`$GOPATH`、および`$GOPATH/bin`にパスを通します。

```bash
# 設定の見やすくするためのヘッダなので無くても良い
echo '' >> ~/.bashrc
echo '# Go Lang' >> ~/.bashrc

# GOPATHの設定
echo 'export GOPATH=$HOME/go' >> ~/.bashrc
echo 'export PATH=$PATH:$GOPATH/bin' >> ~/.bashrc

# .bashrcのリロード
source ~/.bashrc
```

### <参考元>

- [初めてのGo！インストールまで](https://qiita.com/inexp_eng4432/items/08dce692894c92ae08ee)
- [GOPATH は適当に決めて問題ない](https://qiita.com/yuku_t/items/c7ab1b1519825cc2c06f)
- [go run と go buildの違い](http://nununu.hatenablog.jp/entry/2016/09/20/210000)
- [GolangのGOPATHやGOROOTについて](https://tech.librastudio.co.jp/entry/index.php/2018/02/20/post-1792/)
