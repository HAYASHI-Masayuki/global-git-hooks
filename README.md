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
   cd ~/.config/git/hooks
   ./_link_global_git_hooks
   ```

3. Gitのグローバルなフックのパスを設定
   ```sh
   git config --global core.hooksPath ~/.config/git/hooks
   ```


グローバルなGitフックの追加
--------------------------------------------------------------------------------

1. フック名 + `.d`のディレクトリを作成
   ```sh
   mkdir ~/.config/git/hooks/prepare-commit-msg.d
   ```

2. ディレクトリ内に、好きな名前で、実行可能ファイルを設置
   ```sh
   cp .git/hooks/prepare-commit-msg ~/.config/git/hooks/prepare-commit-msg.d/show-latest-commits
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

2. `$XDG_CONFIG_HOME/husky/init.sh`に以下の内容を記載する。
   ```sh
   #!/bin/bash
   if git config core.hooksPath | grep -q '^\.husky'; then
       git config --unset core.hooksPath
       echo Huskyによる変更を修正しました。再実行してください。
       exit 1
   fi
   ```
   8系以前のHuskyを使用している場合には、`~/.huskyrc`にも同様の内容を記載してください。

3. 必要に応じて、`.husky/pre-push`を設置しておく。Huskyの仕様により、設置されていないフックがある場合は初期化スクリプトは動作しないため。

これにより、Husky設定ごとに`git commit`等が一回止まってしまうものの、Huskyとの併用が可能になります。


