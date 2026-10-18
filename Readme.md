
# How to Use Podman Pod File

このリポジトリは、repoツールを使用してPodmanのPodファイルを管理するためのものです。

## 使い方

1. ディレクトリを作成します。

```bash
mkdir file
cd file
```

2. リポジトリをクローンします。

```bash
repo init -u ssh://git@github.com/HNTKAR/podman-pod-file.git -b manifest/main -m 
repo sync
repo start dev  # または main
```

## 注意事項

- ブランチを切り替える際は、作業中の変更をコミットしてから行ってください。
- Podmanのバージョンによっては、一部の機能が動作しない場合があります。
- このリポジトリの内容を使用する際は、必ず最新のPodmanを使用することを推奨します。
