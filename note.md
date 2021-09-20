# Git Note

# 用語集
## origin
* Gitがクローン元のサーバーに対してデフォルトでつける名前

## HEAD


# MEMO

## `git commit -a` 
* -a: 追跡中のファイルの`git add` も同時にしてくれる

## `git status -s(--short)` 
* `git status` の結果表示を簡略化

## `git diff vs git diff --staged(--cached)`
* `git diff`
  * 作業ディレクトリとステージングエリアの比較
* `git diff --staged`
  * ステージングエリアと直近のコミットの比較

## `git rm`
* ファイルの削除をステージングする
  * 作業ディレクトリ上で消しただけでは削除はステージングされない
* 一発で作業ディレクトリからファイルを削除してステージングまでしてくれる

## `git rm --cached <filename>`
* ファイル自体は作業ディレクトリに残しつつgitでは追跡させたくないとき
* まだaddしたくなかったのに間違えてしてしまったとき

## `git mv A.md B.md`
* A.md のファイル名をB.md に変更する
* 以下の処理の結果と同じ
  ```zsh
  $ mv A.md B.md
  $ git rm A.md
  $ git add B.md
  ```

## `git log -p -2`
* -p: 変更点を表示する
* -2: 直近2コミットのみ表示する
* その他のおすすめのオプション
  * --grep=hoge: commitコメントにhogeが含まれているログに絞る
  * -Shoge: hogeというコードを追加・削除したコミットのログに絞る

## `git commit --amend`
* 直前のコミットを上書きする。
* ステージングエリアの内容をコミットに使用する。
* 例えば一度コミットしたあとステージし忘れていたファイルに気づいた場合は以下の通り。
```zsh
# 最初のコミットは２番目のコミットに上書きされる。
$ git comit -m "initial commit"
$ git add forgotten_file.go
$ git commit --amend
```

# 特集：取り消したい
取り消しに関する方法は`git status`が教えてくれる。

## `git reset HEAD <file>...`
* stageしたファイルのうちいくつかをunstageしたい。
* 作業ディレクトリのファイルの変更はそのままにしておきたい。
* `git reset` は --hard をつけない限り安全。
* TODO: `git restore --staged <file>...` も同じ結果になる？

## `git checkout -- <filename>`
* ステージしていないファイルの変更を取り消したい。(直近のコミット時点の状態に戻す)
* このコマンドはファイルの変更を取り消して戻せなくするので要注意。

## まとめ
* コミットした内容の全てはほぼ常に取り消し可能。
  * 削除したブランチへのコミットや--amendコミットで上書きされた元のコミットでさえも。
* しかしまだコミットしていない内容を失ってしまうとそれは二度と戻らない。
* ちょっとでも迷ったら`git status`

# remote編 
## `git remote -v`
* そのリポジトリのクローン元サーバーを表示する。
  * -v: 書き込み用と読み取り用の２つのURLを表示する

## `git remote add hoge git@github.com:yokoyamada/learngit`
* Clone元サーバーのURLに対してhogeというエイリアスを登録する。

## `git fetch [remote-name]`
* リモートプロジェクトのすべてのデータの中から自分が持っていないものを引き出す。
* ローカル環境にマージされたり作業中の内容を書き換えたりすることはない。
* そのため必要に応じて自分でマージをする必要がある。
* どういうこと？他人によってプッシュされた状態で`git fetch`すると、じゃあどうなるの？
  * 自分が編集中のファイルは、コミットの際に初めてマージされる。
  * 自分が編集していないファイルも、作業ディレクトのファイルには反映されず、commit の際に初めてマージされる。
  * merge 後は必ず`git pull`してテストして...という工程が必要だということだな。

## `git pull`
* 自動的にfetchを行い、リモートブランチの内容を現在のブランチにマージする。

## `git push [remote-name] [branch-name]`
* おなじみpush
* 誰かがpushしたあとで自分がpushしようとするとrejectされる。
* rejectされたら、まず誰かがpushした内容を引き出して、ローカルで調整してから出ないとpushできない。

## `git remote show origin`
* remoteの情報をより詳しく知りたいときに使用する。
* remote repository のURL、追跡対象のbranchの情報が表示される。

## `git remote rename A B`
* リモートの名前を変更する。(AをBに変更する)

## `git remote rm B`
* リモートを削除する。
* 多分だけどローカルからリモートサーバーの情報を削除する、という意味だと思う。
