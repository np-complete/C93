# 実際の作業フロー

さて最後に、実際の作業フローをコマンドとともに紹介します。

## 既存のリポジトリをcloneしてフォーク

既存のリポジトリの `clone` は `ghq` で行います。 `ghq get` の引数には、取得したい `ユーザ名/リポジトリ名` の形式で指定します。
例えば `dwango/mastodon` を取得したいなら、

    $ ghq get dwango/mastodon

のように指定します。
mastodonの本家リポジトリである `tootsuite/mastodon` ではないということがポイントです。

これで、 `https://github.com/dwango/mastodon` を `~/src/github.com/dwango/mastodon` に、 `origin` のリモート名で取得できます。

もし、`tootsuite/mastodon` にもプルリクエストしたい場合、同じように `ghq` で取得します。
`dwango/mastodon` とは別のディレクトリ (`~/src/github.com/tootsuite/mastodon`) に `clone` され、**コードが混濁することはありません**。
本家ディレクトリの中で `dwango/mastodon` のリモートを登録したい、などとは考えないほうが得策です。

コードを取得したら、そのディレクトリで

    $ hub fork

を実行します。
すると、 Github上でフォークされ、 リモート名 `masarakki` で `git@github.com:masarakki/mastodon` が登録されます。

つまり、ただフォークしてプルリクエストするだけなら、
自分のディレクトリ (`~/src/github.com/masarakki/mastodon`) を作ることはありません。

`dwango/mastodon` と `tootsuite/mastodon` で同じようにフォークすると、ひとつの `masarakki/mastodon` が共有される形になります。
当然リモートブランチなども共有され、少し複雑になりますが、
後述するプルリクエストは上手く動作するので安心してください。

## 新規作成の場合

リポジトリを新規作成する場合、自分のディレクトリ (`~/src/github.com/masarakki`) に、リポジトリ名でディレクトリを作り、その中で

    $ git init
    $ hub create

を実行します。
すると、**そのディレクトリ名**でリポジトリが作られ、 リモート名 `origin` で `git@github.com:masarakki/リポジトリ名` が登録されます。

ここで、他人のリポジトリをフォークした時と整合性を合わせるため、

    $ git remote rename origin masarakki
    $ git remote add origin https://github.com/masarakki/リポジトリ名

を実行します。
(あるいは他人のフリをして一度 `ghq get` しなおしてもいいでしょう。)

## ブランチを切って開発する

開発ブランチは常に、リポジトリのデフォルトブランチである **HEAD branch** から切ります。

    $ git start new-feature
    # edit, edit, edit
    $ git save
    # edit, edit, edit
    $ git save

とにかくブランチ名の命名だけには脳みそを稼働させましょう。
途中のコミットなんてどうでもいいです。

## プルリクエスト

開発中の早いタイミングでプルリクエストをを出してしまいましょう。
まずブランチをpushします。

    $ git push masarakki new-feature

ここで注目すべきは、 `origin` ではなく `masarakki` に `push` していることです。
たとえ、自分自身が所有するプロジェクトで、`origin` に対するpush権限があったとしても、必ず個人アカウントのリポジトリにpushします。

その後、

    $ hub pull-request

を実行すると、エディタが開き、プルリクエストを作成できます。
まだ開発中の場合は、**[WIP]**を付けるのを忘れないでください。
プルリクエストは、デフォルトで `origin` のHEAD branchに対して行われます。
そのため、`ghq` コマンドの流儀と `hub` コマンドの動作に従えば、
`dwango/mastodon` の開発と `tootsuite/mastodon` の開発を混濁せずに同時進行で行えます。

ここまでの手法は、**自分がオーナーのリポジトリ**でも、**自分がグループメンバーのリポジトリ**でも、**全く他人のリポジトリ**でも**全く同じ手順**で行えます。

## プルリクエストのレビューを受ける

開発途中から、積極的にレビューを受けましょう。
そしてリポジトリには、**必ずCIを設定**し、自動テストします。
テストが通り、レビューが通ったらマージの準備ができます。

    $ git squash
    $ git push -f masarakki new-feature

### プルリクエストをマージする

デフォルトブランチのものはいつでもデプロイできるという大原則があります。
マージ直前のこのタイミングが、間違ったコードを入れてしまわないための最後のチャンスです。

もちろんテストが落ちていたらマージしてはいけません。
テストコードが十分でない場合も大問題です。
レビューはしましたか? もう大丈夫ですか?
実際に動かしてみましたか?

今一度確認して、本当の本当に大丈夫だったらマージボタンを押しましょう。
