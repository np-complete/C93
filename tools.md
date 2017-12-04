## ツール

ここで読者のみんなにイカれたツールたちを紹介するぜ!
リポジトリは [masarakki/yabai-git-commands](https://github.com/masarakki/yabai-git-commands) だ!!

### git start

    $ git start new-branch-name

このコマンドは、指定された名前でブランチを作り、ブランチ名と同じコミットメッセージで空コミットするコマンドです。
ステージされた変更があればからコミットではなく普通のコミットになります。
「ブランチ名とコミットメッセージが同じ」を最速で実現します。

### git save

    $ git save

このコマンドは、`git commit` の代わりに使うコマンドで、`fixup! ブランチ名` というメッセージでコミットします。
つまり、

    $ git start new-branch
    $ git save
    $ git save

とやると、

- new-branch
- fixup! new-branch
- fixup! new-branch

というコミットが積み重なっていきます。

### git squash

    $ git squash

このコマンドは、squashするためのコマンドですが、前述の fixup! の魔法が上手く作用し、自動で全てのコミットをひとつにまとめてくれます。

この3つのコマンドにより、
`git start` でブランチを作り、変更は適度なタイミングで `git save` し、最後 `git squash` してプルリクエストを出す、というのが**圧倒的に雑で省エネで脳みそ負荷ゼロ**で行えます。
**アタマを使って考える**のはブランチ名だけです。
これが怠惰な人間が本気で作ったGitワークフローの真髄です。

### おまけ

ここに入れるかどうか迷いましたが、[masarakki/dotfiles](https://github.com/masarakki/dotfiles) にもいろいろ便利なGit拡張コマンドを収録しているのでご覧ください[^1]。

[^1]: 今回のコマンドもここから分離独立させたものです
