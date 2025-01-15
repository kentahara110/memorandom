# zip圧縮コマンド
## 概要
macの.DSファイルを含めずzip圧縮するコマンド
圧縮したいファイルやフォルダがあるディレクトリで下記コマンドを実行

```shell
% zip name.zip -r name/ -x "*.DS_Store"
```