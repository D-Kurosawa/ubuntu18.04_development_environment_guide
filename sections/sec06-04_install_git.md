# Git のインストール

## 1. Git のイントール

Gitは開発だけではなくパッケージのインストール等いろいろな場面で使用するので、以下のコマンドを実行してインストールします。

```bash
sudo apt install git
```

## 2. Git の設定

Gitの基本的な設定は以下のコマンドで行います。

```bash
git config --global user.name [ユーザー名]
git config --global user.email [メールアドレス]
git config --global color.ui "auto"
```

他にも設定項目はありますが、ここではGitをイントールして使用することを目的としているので、aliasやエディタ設定等の使い易さを向上させる設定はここでは取り扱いません。

### <Gitの設定に関する参考先>

- [Git公式 Gitの設定](https://git-scm.com/book/ja/v1/Git-%E3%81%AE%E3%82%AB%E3%82%B9%E3%82%BF%E3%83%9E%E3%82%A4%E3%82%BA-Git-%E3%81%AE%E8%A8%AD%E5%AE%9A)

- [Gitをインストールしたら真っ先にやっておくべき初期設定](https://qiita.com/wnoguchi/items/f7358a227dfe2640cce3)

## 3. Git の設定確認

Gitの設定は以下のコマンドで確認できます。

```bash
git config --global --list
```

また、Gitの設定情報は以下のファイルに保存されるのでエディタでの直接編集も可能です。

**`~/.gitconfig`**
