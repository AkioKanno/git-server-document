**Gitサーバ構築メモ**  
恥ずかしながら、今までgit hub, git laboにて開発は行ってきた経験はあるが、  
サーバにgit のリポジトリ構築経験がありませんでした。  
  
そこで今回は適当なサーバに リポジトリを作成し、そこでgit 開発が  
行えるまでのメモを書いていきます。  
  
#  1.git のインストール  
     yum install git  
  
#  2. git daemon のインストール  
     yum install git-daemon  
  
#  3. リポジトリの作成  
    su git  
    cd /var/lib/git  
    mkdir (リポジトリ名)  
    cd (リポジトリ名)  
    git init --bare --shared=true  

※point : リポジトリ作成は git ユーザとか作成して行うとよい感じです  
  
#  4. git-daemonの起動  
    /usr/bin/git-daemon --base-path=/var/lib/git/ --export-all --enable=receive-pack  --reuseaddr --verbose /var/lib/git  
※point : push などのを制限するのであれば"--enable=receive-pack"のオプションは外してください  
  
上記のコマンドを叩くと帰ってこないので、閉じないで別窓などを立ち上げてください  
  
#  5. cloneしてみる  
    cd (適当な開発ディレクトリへ移動)  
    git clone git://localhost/リポジトリ名  
実行後、git-daemon の画面にログが流れます  
私は リポジトリのパーミッションにより、失敗しておりました....  
  

#  6. pushしてみる  
ここまでくればあと一歩！ ゴールは目前です。  

    echo "test !" > test.txt  
    git add test.txt  
    git commit -m "first commit"  
    git config --global user.name "名前"  
    git config --global user.email "メアド"  
    git remote add origin git://localhost/リポジトリ名称  
    git push -u origin master  
  
実行後、以下のそれっぽい文言が出ていれば完了です  
  Counting objects: 3, done.
  Writing objects: 100% (3/3), 216 bytes, done.
  Total 3 (delta 0), reused 0 (delta 0)
  To git://10.21.68.208/internal.systems
   * [new branch]      master -> master
  Branch master set up to track remote branch master from origin.
  
