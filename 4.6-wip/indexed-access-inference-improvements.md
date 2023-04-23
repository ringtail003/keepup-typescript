# Indexed Access Inference Improvements

## TL;DR

オブジェクトがクロージャに渡った時の型推論が正しく動作するようになった。

## 前提

ログ書き込みをサンプルとする。

{% code overflow="wrap" %}
```typescript
interface LogOperation {
  loggedIn: { 
    date: Date;
    userName: string;
  };
  switchRole: {
    beforeRole: string;
    afterRole: string;
  };
}

```
{% endcode %}

ログ書き込み部分を共通化し、テキストと整形をパラメータで渡す。

```typescript
/* ==================================================
  "ringtail003 logged in at 2023/01/31 10:11:12"
================================================== */
writeLog({
  operation: 'loggedIn',
  value: {
    date: new Date(),
    userName: 'ringtail003',
  },
  format: (value) => 
    `${value.userName} logged in at ${value.date.toLocaleString()}`,
});

/* ==================================================
  "Switch STAFF to ADMIN"
================================================== */
writeLog({
  operation: 'switchRole',
  value: {
    beforeRole: 'staff',
    afterRole: 'admin',
  },
  format: (value) => 
    `Switch ${value.beforeRole} to ${value.afterRole}`,
});
```

共通化のコードは以下のもの。肝は `operation` で「操作」を特定し、`format` に該当する「値」が渡ることにある。

```typescript
function writeLog<K extends keyof LogOperation>(log: LogFormat<K>) {
  fs.writeFileSync(
    'yyyy-MM-dd.log',
    log.format(log.value)
  );
}

type LogFormat<P extends keyof LogOperation> = {
  [K in P]: {
    operation: K; // <===== loggedIn | switchRole が入る
    value: LogOperation[K];
    format: (p: LogOperation[K]) => void; // <===== loggedIn なら date を持っている
  };
}[P];
```

## Previous

クロージャのパラメータの型推論が弱かった。

```typescript
writeLog({
  kind: 'loggedIn',
  value: ...,
  format: (value) => 
    `${value.userName} logged in at ${value.date.toLocaleString()}`,
    // ❌ ERROR! userName does not exists
    // ❌ ERROR! date does not exists
});
```

## Current

クロージャのパラメータが正しく推論されるようになった。

```typescript
writeLog({
  kind: 'loggedIn',
  value: ...,
  format: (value) => 
    `${value.userName} logged in at ${value.date.toLocaleString()}`,
    // ✅ PASS! userName: string
    // ✅ PASS! date: Date
});
```
