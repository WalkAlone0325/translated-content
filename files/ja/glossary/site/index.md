---
title: Site (サイト)
slug: Glossary/Site
tags:
  - Glossary
  - Security
  - WebMechanics
  - 用語集
translation_of: Glossary/Site
---
ウェブコンテンツの一部であるい*サイト*は、オリジン内にあるホストの*登録可能なドメイン*によって決定されます。これは、*公開接尾辞リスト*を参照して、*公開接尾辞*として数えられるホストの部分を見つけることによって計算されます (e.g. `com`, `org`, `co.uk`)。

*サイト*の概念は、ウェブアプリケーションの [Cross-Origin Resource Policy](</ja/docs/Web/HTTP/Cross-Origin_Resource_Policy_(CORP)>) と同様に、 [SameSite クッキー](/ja/docs/Web/HTTP/Headers/Set-Cookie#Directives)で使用されています。

## 同一サイトの例

| `https://developer.mozilla.org/en-US/docs/` `https://support.mozilla.org/en-US/` | 登録可能なドメイン _mozilla.org_ が同じなので同一サイト |
| -------------------------------------------------------------------------------- | ------------------------------------------------------- |
| `http://example.com:8080` `https://example.com`                                  | スキームとポートは関係ないので同一サイト                |

## 異なるサイトの例

| `https://developer.mozilla.org/ja/docs/` `https://example.com` | 2 つの URL の登録可能ドメインが異なるため、同一サイトではない |
| -------------------------------------------------------------- | ------------------------------------------------------------- |

## 仕様書

| 仕様書                                               | 状態                 | 備考     |
| ---------------------------------------------------- | -------------------- | -------- |
| {{SpecName('URL', '#host-same-site')}} | {{Spec2('URL')}} | 初回定義 |