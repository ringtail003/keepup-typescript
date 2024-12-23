# Consulting package.json Dependencies for Declaration File Generation

## TL;DR

{% code overflow="wrap" %}
```
The inferred type of "X" cannot be named without a reference to "Y". This is likely not portable. A type annotation is necessary.
```
{% endcode %}

従来のバージョンでは明示的にインポートされていないファイルで、型が解決できず上記エラーが発生していた。5.5からpackage.jsonのdependencies、peerDependencies、optionalDependenciesの依存関係を追跡するようになり、エラーが解消した。
