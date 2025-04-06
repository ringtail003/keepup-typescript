# Optimizations on Program Loads and Updates

importのパスの正規化でパフォーマンスが改善された。

具体的には、パスを配列に分離して文字列で再構築する部分へのアプローチ。\
またtsconfig.jsonなどプロジェクトの構成に関与しないファイルの変更で、パスの再構築が走らないようになった。
