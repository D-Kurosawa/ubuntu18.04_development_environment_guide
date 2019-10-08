# ブラウザのインストール

## 1. Google Chromeのインストール（deb版）

Ubuntuには`Fire Fox`がデフォルトでインストールされてますが、`Google Chrome`を普段から利用しているので、使い勝手という観点から`Google Chrome`をインストールします。  \
最も簡単な方法はGUIから`Fire Fox`を起動してChromeを検索してイントールする方法です。リポジトリも登録されるのでアップデートもできます。

## 2. Google Chromeのインストール（apt版）

`apt`リポジトリとして`Google Chrome`を追加するには、以下のコマンドを実行します。

```bash
# コピペを簡単に行うために、ターミナルの「$」は省略します

# apt リポジトリ
wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | sudo apt-key add -
sudo sh -c 'echo "deb [arch=amd64] http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google.list'

sudo apt update

# インストール
sudo apt install google-chrome-stable
```
