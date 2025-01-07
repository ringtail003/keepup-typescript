# Allow --build with Intermediate Errors

## TL;DR

`--build` について、プロジェクトBがプロジェクトAに依存する時、プロジェクトAにコンパイルエラーがあってもプロジェクトBがコンパイルできるよう変更された。

コンパイルをストップしたい時は `--stopOnBuildErrors`を用いる。
