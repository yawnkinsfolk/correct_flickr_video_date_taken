correct_flickr_video_date_taken
===============================

flickr動画の"でたらめ"な撮影日時を修正するスクリプト
------
This tool corrects "date taken" of flickr video upload date to actual taken date.  
公式のものを含むほとんどのflickrアップローダは動画(video)をアップロードした際に正しい撮影日時(date taken)を設定してくれず、flickr側のデフォルトの挙動である北米時間(-08:00)でのアップロード日時が設定されてしまいます。
このスクリプトは、flickr上にアップロード済みの動画の撮影日時を、対応するローカルの動画ファイルの更新日時に書き換えることで正しい撮影日時に修正します。

flickr動画のタイトルが元ファイルの名前（拡張子は有無どちらでも）になっていることを前提としています。動画アップロードが可能なほとんどのアップローダはそのような命名をします。

撮影日時の修正対象は動画ファイルのみです。写真についてはflickrがExifの撮影日時を読むので動画のような問題が生じません。

このスクリプト自身にアップロード機能はありません。使わざるを得ない既存アップローダの問題点を回避するためのツールです。自分自身がEye-Fiを快適に使うためにこのようなものが必要でした。

動作環境
------
+ 撮影日時を修正したい動画がアップロードされている、あなたの[flickr](http://www.flickr.com/)アカウント
+ Microsoft Windows　(7 64bitでのみ動作確認しています)
+ Ruby　([RubyInstaller](http://rubyinstaller.org/)でインストールしたRuby 1.9.3でのみ動作確認しています)
    + インストール時に「Ruby の実行ファイルへ環境変数 PATH を設定する」と「.rb と .rbw ファイルを Ruby に関連づける」にチェックを入れることをおすすめします。
+ [flickraw](https://github.com/hanklords/flickraw)　(Rubyライブラリ(gem)。インストール方法はflickrawのGitHubページなどを参考にしてください。)
 
使い方
------
+ `correct_flickr_video_date_taken.rb`
+ `crypt.rb`
+ `webbrowser.rb`

の3ファイルをダウンロードして任意のディレクトリに置いてください。GemfileとGemfile.lockは必須ではありません(Bundler用です)。

コマンドプロンプトで
` correct_flickr_video_date_taken.rb [media_path1] [media_path2]... `
を実行してください。

`media_path` には、flickrにアップロードした動画の元ファイルが入っているディレクトリを指定してください。指定したディレクトリ以下のサブディレクトリも含む全てのファイルを走査します。`media_path`を省略した場合にはスクリプトを実行したディレクトリが指定されたものとして扱います。

初回実行時にはflickr側での認証が必要となります。ブラウザのページが開くのでflickrにログインし、このアプリ（スクリプト）の認証(authorize)をお願いします。表示されるコードをコピーしてコマンドプロンプト上のカーソル行にペーストしEnterしてください。認証用情報をファイル(`tok.enc`, `sec.enc`)に保存するので次回以降は認証不要です。

flickr側はアップロード済みのすべての動画が対象となります。
 
flickr側の撮影日付(date taken)が変更される条件
----------------
+ 動画系の拡張子が付いたファイルが走査対象のディレクトリ内にある
    + 対象拡張子は avi,wmv,mov,mpeg,mpg,m4v,mp4,3gp,m2ts,mts,ogg,ogv です。
+ そのファイルの名前（拡張子付きか拡張子なしのいずれか）と同一のタイトルを持った動画(video)がflickrアカウント内に存在する
+ ローカルファイルとflickrコンテンツの長さ(秒単位)が一致している
+ ローカルファイルのプロパティ「フレーム率」を読み出し、値が返ってくることを確認（対象ファイルが動画であることのチェック）
+ flickrのdate takenとローカルファイルの更新日時が異なる

以上の条件を全て満たした場合に、ローカルファイルの更新日時がflickr側の撮影日時(date taken)に書き込まれます。一つのローカルファイルに対してflickr側に上記条件を満たす動画が複数ある場合、すべての撮影日時が書き換えられます。

アンインストール
------
+ `correct_flickr_video_date_taken.rb`
+ `crypt.rb`
+ `webbrowser.rb`
+ `tok.enc`（初回認証後に作られます）
+ `sec.enc`（初回認証後に作られます）

を削除し、flickrアカウント上で本アプリ（スクリプト）との連携を解除してください。

参考情報:
------
自分が知っている、動画に正しい撮影日時を付けてくれるflickrアップローダ

+ CameraSync(iOS)
+ Adobe Photoshop LightroomのJeffrey’s “Export to Flickr” Lightroom Plugin(Win,Mac)
+ clipstart(Mac)　未確認
+ flickfolio(Android)　未確認


謝辞
------
+ [flickraw](https://github.com/hanklords/flickraw)　これが良さそうだったのでRubyで書くことにしました。
+ [RubyでデフォルトのWebブラウザを開く](http://blog.monoweb.info/blog/2012/03/06/ruby-web-browser/)　ブラウザを開くコードとして使わせていただきました。&を含むURLを渡すために少し改造しました。
+ [Ruby スクリプトでデータを暗号化する方法](http://webos-goodies.jp/archives/encryption_in_ruby.html)　認証用トークン保存時の暗号化のコードとして使わせていただきました。
+ [GitHub Preview](http://github-preview.herokuapp.com/)　この文書を作成するために使わせていただきました。
+ [脱GitHub初心者を目指す人のREADMEマークダウン使いこなし術](http://tokkono.cute.coocan.jp/blog/slow/index.php/programming/markdown-skills-for-github-beginners/)　この文書の雛形として使わせてもらいました。

その他、コードを書くにあたって多数のサイトを参考にさせていただきました。


ライセンス
----------
Copyright &copy; 2014 Obata Miki(小畑 幹)  
Licensed under the [MIT License][MIT].

[MIT]: http://www.opensource.org/licenses/mit-license.php