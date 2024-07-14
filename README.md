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


