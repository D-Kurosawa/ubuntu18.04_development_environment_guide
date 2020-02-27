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

一覧に既存の公開鍵と秘密鍵のペア (`id_rsa.pub`と`id_rsa`など) があり、それをGitHubへの接続に利用したい場合、SSHキーを`ssh-agent`に追加します。

## <参考元>

- [GitHub公式](https://help.github.com/ja/github/authenticating-to-github/connecting-to-github-with-ssh)
- [GitHubでssh接続する手順 ~公開鍵・秘密鍵の生成から~](https://qiita.com/shizuma/items/2b2f873a0034839e47ce)
- [GitHubにssh接続できるようにする](https://qiita.com/0ta2/items/25c27d447378b13a1ac3)
