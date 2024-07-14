グローバルなGitフック
================================================================================

使い方
--------------------------------------------------------------------------------

1. このリポジトリをcloneする
   ```sh
   git clone https://github.com/HAYASHI-Masayuki/global-git-hooks.git ~/.config/git/hooks
   ```

2. シンボリックリンクを生成
   ```sh
   ./_generate_git_ignore
   ```

3. Gitのグローバルなフックのパスを設定
   ```sh
   git config --global core.hooksPath ~/.config/git/hooks
   ```


グローバルなGitフックの追加
--------------------------------------------------------------------------------

1. フック名 + `.d`のディレクトリを作成
   ```sh
   mkdir prepare-commit-msg.d
   ```

2. ディレクトリ内に、好きな名前で、実行可能ファイルを設置
   ```sh
   cp .git/hooks/prepare-commit-mgs ~/.config/git/hooks/prepare-commit-msg.d/show-latest-commits
   ```


Huskyとの併用
--------------------------------------------------------------------------------

リポジトリごとのGitフックを実現する[Husky](https://typicode.github.io/husky/)は便利ですが、既存のGitフックを無視し、またその機能の実現のためにリポジトリごとの`git core.hooksPath`を書き換えるため、グローバルなGitフックとの相性が悪いです。

以下の2つの設定により、グローバルなフック、ローカルなフックを残したままHuskyによるリポジトリごとのフックを動作させることができます。

1. 環境変数`USING_HUSKY`を設定する
   ```sh
   # .zprofile等で
   export USING_HUSKY=1
   ```

2. `~/.huskyrc`(最近は`$XDG_CONFIG_HOME/husky/init.sh`等の方がよい？)に以下の内容を記載する。
   ```sh
   #!/bin/bash
   if [ $(git config core.hooksPath) = '.husky' ]; then
       git config --unset core.hooksPath
       echo Huskyによる変更を修正しました。再実行してください。
       exit 1
   fi
   ```

これにより、Husky設定ごとに`git commit`等が一回止まってしまうものの、Huskyとの併用が可能になります。


