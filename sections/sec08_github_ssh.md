# GitHub へのSSH接続

SSHプロトコルを利用すれば、リモートのサーバーやサービスに接続し、認証を受けられます。

SSHキーがあれば、ユーザ名やパスワードをアクセスのたびに入力することなく GitHubに接続できます。

## 1. 既存のSSHキーの確認

SSHキーを生成する前に、SSHキーがすでに存在するかどうかを以下のコマンドで確認できます。

```bash
ls -al ~/.ssh
```

ディレクトリの一覧から、公開SSHキーをすでに持っているか確認します。デフォルトでは、公開鍵のファイル名は以下のいずれかです。

- id_rsa.pub
- id_ecdsa.pub
- id_ed25519.pub

公開鍵と秘密鍵のペアが存在しないか、既存の鍵をGitHubへの接続に利用したくない場合は新しい SSHキーを作成します。

一覧に既存の公開鍵と秘密鍵のペア(`id_rsa.pub`と`id_rsa`など)があり、それをGitHubへの接続に利用したい場合、SSHキーを`ssh-agent`に追加します。

## 2. 新しいSSHキーを生成

以下のコマンドを実行します。メールアドレスは自分の`GitHub`メールアドレスに置き換えてください。

```bash
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

何か聞かれたら3回エンターを押せば、id_rsaとid_rsa.pubの2つの鍵が生成されます。なお、2回目、3回目はパラフレーズの設定（パスワード設定のようなもの）ですが、ここはなくて大丈夫です。

**既にid_rsaが存在する場合は上書きされてしまうので注意です。**

違う名前で生成したい場合は以下のコマンドを実行します。

```bash
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
Generating public/private rsa key pair.
Enter file in which to save the key (/Users/(username)/.ssh/id_rsa):id_git_rsa
```

一回目に聞かれたタイミングで名前(ここではid_git_rsa)を指定。あとはエンターでid_git_rsaとid_git_rsa.pubが生成されます。

ちなみに、名前を指定しない時は勝手に`~/.ssh`内に鍵を生成してくれますが、上記のように名前を指定するときは`~/.ssh`内にいないとこの中に生成してくれないので注意です。

### 3. SSHキーをssh-agentに追加

以下のコマンドでバックグラウンドで`ssh-agent`を開始します。

```bash
eval "$(ssh-agent -s)"
```

SSHプライベートキーを`ssh-agent`に追加します。キーを別の名前で作成したか、別の名前を持つ既存のキーを追加しようとしている場合は、コマンド内の`id_rsa`を秘密鍵ファイルの名前で置き換えてください。

```bash
ssh-add ~/.ssh/id_rsa
```

## <参考元>

- [GitHub公式](https://help.github.com/ja/github/authenticating-to-github/connecting-to-github-with-ssh)
- [GitHubでssh接続する手順 ~公開鍵・秘密鍵の生成から~](https://qiita.com/shizuma/items/2b2f873a0034839e47ce)
- [GitHubにssh接続できるようにする](https://qiita.com/0ta2/items/25c27d447378b13a1ac3)
