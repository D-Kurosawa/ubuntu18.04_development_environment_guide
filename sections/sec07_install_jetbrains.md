# JetBrains

JetBrainsは、チェコ共和国の首都プラハに本社を置く技術主導型のソフトウェア開発企業です。主要製品として、統合開発環境(IDE)が有名である。同社は、OSによらずJava仮想マシン上で動くオブジェクト指向プログラミング言語**Kotlin**も開発した。

現時点では、JetBrains社製のIDEを使用しておけば、ほぼ間違いないと言われるほど多機能である。JavaのIDEであるIntelliJ IDEAは最も人気のIDEである。また、PythonのIDEであるPyCharmも機能の豊富さから人気がある。

なお、JetBrains社のIDEは言語別になっており、各IDEに対してライセンス料が発生する。`All Products Pack`ライセンス契約では**JetBrains社のデスクトップ製品を全て利用可能**となるので複数言語のIDEを使用したい場合はこちらの方が得である。

## ToolBox App のイントール

JetBrains社のIDEは言語によってIDEが分離されているが、`Toolbox`を利用すると**JetBrains社のToolbox製品(=IDE製品)の起動、インストール、更新および設定を一元管理することができます。

まず、ブラウザで`ToolBox App`をダウンロードします。

次に、公式のガイドに従い、以下のコマンドでToolBox Appをイントールします。最後に、`/opt/jetbrains-toolbox-*`内のバイナリを起動してインストール完了です。

```bash
cd ~/Downloads/

# *はバージョン情報
sudo tar -xvzf jetbrains-toolbox-*.tar.gz -C /opt

cd /opt/jetbrains-toolbox-*/
./jetbrains-toolbox
```

## <参考元>
