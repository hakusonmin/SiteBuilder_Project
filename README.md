## 概要
・AnnaiさんのDrupalスタートブックを元に作ったサイトである。
そして、以下の記述はANNAIさんの書籍の内容に加えて独自に行ったものについてだ。

## 工夫した点

### バージョンの差異において工夫した点
   ・Drupal8系の頃の書籍のため、Drupal10系ではうまくいかないところがあった。具体的には、
   モジュールやテーマのダウンロードにcomposerを利用した。
   フロントページへ掲載がデフォルトでオンになっていたので変更した。
   drush ランチャーが廃止になってしまったため ~/.zshrcに alias drush='vendor/bin/drush'
   を書いておくことによって毎回フルパスでコマンドを打つ必要がないようにした。などが挙げられる。

### 環境構築において工夫した点
・Annaiさんの書籍ではクラウドを利用しローカル環境の構築方法が記載されていなかったため、
「Drupal9 WEB開発ことはじめ」というDrupal Meetup 豊田支部さん の書かれた書籍を利用させていただいた。

・最初は
```
$ composer create-project drupal/recommended-project my-drupal
$ cd my-drupal
$ php web/core/scripts/drupal quick-start standard 
```
の形式で利用していたのだが起動に失敗するので公式ドキュメントを読み memory不足を解消するオプションを追加し,
また複数のローカル環境を作るためport番号を指定した。それが
```
$ php -d memory_limit=256M web/core/scripts/drupal quick-start standard --langcode ja --port=xxxx
```
この形式である。

・さらにメモリ指定したにもかかわらず、途中でサイトが止まってしまうため、
２回目以降の起動は 
```
$ php -S localhost:xxxx .ht.router.php
``` 
を利用した、
(ddevはdocker scotが起動してしまい 急激に重くなってしまったので断念した。)
(landoは単体で利用する分には問題なかったが、複数Landoを利用するとかなり重くなってしまった。)
そのため現在はローカルのquick-startもしくは純粋なdockerによるサーバーの起動を行っている。

### その他
・ベータ版のprojectbrowerとnavigation top bar を利用をした。
projectbowserは256のメモリがないと動かなかった。 navigation top bar は admin extra toolsが
使えなくなるので困った。

・リレーションシップを用いて、製品一覧ページに投稿者の名前が出るようにした。
・またコンテクスチュアルフィルターを用いることによってタクソノミーが関連する記事のブロックを作成した。

・製品紹介ページや製品紹介ページのViewブロックがずれて表示されていたため、Crop APIを利用して
トリミングを行おうとしたが、問題はそこではなく表示管理の画像スタイルであった。
変更前は画像スタイルにオリジナル画像が選択されていたため、はみ出てしまっていたのだ。
そのため新たに適切なサイズのカスタム画像スタイルを作成し、それぞれ適用させた。

### 使用したモジュール
・admin toobar /admin toobar extra tools キャッシュクリアが簡単になった。<br>
・devel コンテンツを自動生成する用途に使用した。またユーザーの切り替えが便利である。<br>
・Crop API/Image Widget Crop/Focal Point トリミングに使用した。<br>
・Pathauto タクソノミータームのpathを綺麗にするために利用した。
一括でエイリアスを更新する機能が有用であった。<br>
・webprofilerでサイトに登録されているすべてのルートを確認した。<br>
・simple environment indicator を利用した。 現在は無効済み。<br>
 