---
title: 攻撃の種類
slug: Web/Security/Types_of_attacks
translation_of: Web/Security/Types_of_attacks
---
この記事では、様々な種類のセキュリティ攻撃とそれを軽減するためのテクニックについて解説しています。

## クリックジャッキング

クリックジャッキングとは、ユーザーを騙して、ユーザーが思っているものとは別のリンクやボタンなどをクリックさせることです。これは、例えば、ログイン認証情報を盗んだり、マルウェアの一部をインストールするためにユーザーの無意識のうちに許可を得るために使用することができます。(クリックジャッキングは、「ユーザーインターフェースのリドレス」と呼ばれることもありますが、これは「リドレス」という用語の誤用です)。

## クロスサイトスクリプティング (XSS)

クロスサイトスクリプティング（XSS）は、攻撃者が悪意のあるクライアント側のコードをウェブサイトに注入することができるセキュリティエクスプロイトです。このコードは被害者によって実行され、攻撃者はアクセス制御を迂回したり、ユーザーになりすましたりすることができます。Open Web Application Security Project によると、XSS は 2017 年に[7 番目に多いウェブアプリの脆弱性](https://owasp.org/www-project-top-ten/2017/Top_10)だったという。

これらの攻撃は、ウェブアプリが十分な検証やエンコーディングを採用していない場合に成功します。ユーザーのブラウザーは、悪意のあるスクリプトが信頼できないものであることを検出できないため、クッキー、セッション トークン、またはその他のサイト固有の機密情報へのアクセスを許可したり、悪意のあるスクリプトに {{glossary("HTML")}} コンテンツを書き換えさせたりします。

クロスサイトスクリプティング攻撃は、通常、1) 信頼されていないソース (多くの場合、ウェブリクエスト) を介してウェブアプリにデータが入力されたり、2) 悪意のあるコンテンツが検証されずに動的なコンテンツがウェブユーザーに送信されたりした場合に発生します。

悪意のあるコンテンツには、{{glossary("JavaScript")}} が含まれていることが多いですが、HTML、Flash、またはブラウザーが実行できるその他のコードが含まれていることもあります。XSS に基づく攻撃の種類はほぼ無限にありますが、一般的には、クッキーなどのセッション情報などのプライベートデータを攻撃者に送信したり、攻撃者が管理するウェブページに被害者をリダイレクトさせたり、脆弱性のあるサイトを装ってユーザーのマシンに悪意のある操作を行ったりすることが多いです。

XSS 攻撃は、蓄積型 (パーシステントとも呼ばれる)、反映型 (非パーシステントとも呼ばれる)、DOM ベースの 3 つに分類されます。

- 格納された XSS 攻撃
  - : 注入されたスクリプトは、ターゲットのサーバーに永久に保存されます。そして、被害者は、ブラウザーがデータの要求を送信すると、この悪意のあるスクリプトをサーバーから取得します。
- 反映された XSS 攻撃
  - : ユーザーがだまされて悪意のあるリンクをクリックしたり、特別に細工されたフォームを送信したり、悪意のあるサイトを閲覧したりすると、注入されたコードは脆弱性のあるウェブサイトに移動します。ウェブサーバーは、注入されたスクリプトを、エラーメッセージ、検索結果、またはリクエストの一部としてサーバーに送信されたデータを含むその他の応答など、ユーザーのブラウザーに反映させます。ブラウザーは、レスポンスがユーザーが既にやり取りしたことのある「信頼できる」サーバーからのものであると想定しているため、コードを実行します。
- DOM ベースの XSS 攻撃
  - : ペイロードは、元のクライアントサイドスクリプトが使用していた DOM 環境 (被害者のブラウザー内) を変更した結果、実行されます。つまり、ページ自体は変わりませんが、DOM 環境を悪意を持って改変した結果、ページに含まれるクライアント側のコードが予期せぬ形で実行される。

## クロスサイトリクエストフォージェリー (CSRF)

CSRF (XSRF とも呼ぶ) は、関連するクラスの攻撃です。攻撃者はユーザーのブラウザーにユーザーの同意なく、または、知らないうちにウェブサイトのバックエンドへの要求を実行させます。攻撃者は XSS ペイロードを使用して CSRF 攻撃を開始できます。

Wikipedia には、CSRF の良い例が記載されています。この状況では、誰かが (例えば、フィルタリングされていないチャットやフォーラムで) 実際には画像ではない画像を含むが、その代わりに、それは本当にお金を引き出すためにあなたの銀行のサーバーへのリクエストです。

```html
<img src="https://bank.example.com/withdraw?account=bob&amount=1000000&for=mallory">
```

さて、あなたが銀行口座にログインしていて、クッキーがまだ有効であれば (他の検証はありません)、この画像を含む HTML を読み込むとすぐに送金されます。POST リクエストを必要とするエンドポイントでは、ページが読み込まれたときにプログラム的に \<form> の送信をトリガーすることができます (おそらく不可視の \<iframe> で)。

```html
<form action="https://bank.example.com/withdraw" method="POST">
  <input type="hidden" name="account" value="bob">
  <input type="hidden" name="amount" value="1000000">
  <input type="hidden" name="for" value="mallory">
</form>
<script>window.addEventListener('DOMContentLoaded', (e) => { document.querySelector('form').submit(); }</script>
```

これを防ぐためには、いくつかのテクニックがあります。

- GET エンドポイントは、変更を実行するアクションであって、データを取得するだけのものではなく、POST (または他の HTTP メソッド) リクエストの送信を必要とするものでなければなりません。POST エンドポイントは、クエリ文字列内のパラメータを持つ GET リクエストを相互に受け入れてはいけません
- CSRF トークンは、非表示の入力フィールドを経由して \<form> 要素に含まれなければなりません。このトークンはユーザーごとに一意で、リクエストが送られたときにサーバーが期待値を調べることができるように、(たとえばクッキーに) 保存されるべきです。アクションを実行する可能性のあるすべての非 GET リクエストに対して、 この入力フィールドは期待値と比較されるべきです。不一致があった場合、リクエストは中止されるべきです
- CSRF トークンを予測できないことに依存しています。トークンはサインイン時に再生成されなければなりません
- 機密性の高い動作 (セッションクッキーなど) に使用されるクッキーは、SameSite 属性を Strict または Lax に設定して、有効期限を短くする必要があります (上記の SameSite cookies を参照)。サポートしているブラウザーでは、これはクロスサイトリクエストと一緒にセッションクッキーが送信されないようにする効果があり、そのリクエストはアプリケーションサーバーに対して事実上認証されません
- CSRF トークンと SameSite Cookie の両方を導入する必要があります。これにより、すべてのブラウザーが保護され、SameSite のクッキーでは対応できない場合 (別のサブドメインからの攻撃など) にも保護されます

より多くの予防のヒントについては、OWASP CSRF prevention チートシートを参照してください。

## 中間者攻撃 (MitM)

第三者がウェブサーバーとクライアント（ブラウザー）間のトラフィックを傍受し、ウェブサーバーになりすましてデータ（ログイン情報やクレジットカード情報など）を取得します。トラフィックは、場合によっては変更された状態で通過します。オープン WiFi ネットワークは、この攻撃を実行する一般的な手段です。

## セッションハイジャック

セッションハイジャックは、ユーザーの認証されたセッションにアクセスして悪用することです。これは、既存のセッションのクッキーを盗んだり、ユーザー (またはブラウザー) を騙して、あらかじめ設定されたセッション ID のクッキーを設定させることで起こる可能性があります。

厳密な Content-Security-Policy を展開することで、侵入経路を制限することができます。

### セッションの固定

第三者は、ユーザーのセッション識別子を判断することができ（つまり、読み取ったり、設定したりすることで）、そのユーザーとしてサーバーとやりとりをすることができます。クッキーを盗むことは、これを行う一つの方法です。

application.example.com のようなサブドメインは、`Domain` 属性を設定することで、example.com や他のサブドメインへのリクエストと一緒に送信されるクッキーを設定できることを思い出してください。

```
Set-Cookie: CSRF=e8b667; Secure; Domain=example.com
```

脆弱性のあるアプリケーションがサブドメイン上で利用可能な場合、このメカニズムはセッション固定化攻撃で悪用される可能性があります。ユーザーが親ドメイン(または別のサブドメイン)のページを訪問したとき、アプリケーションはユーザーのクッキーで送られた既存の値を信頼するかもしれません。これにより、攻撃者は CSRF 保護を迂回したり、ユーザーがログインした後にセッションを乗っ取ったりすることができます。
あるいは、親ドメインが `includeSubdomains` が設定された [HTTP Strict-Transport-Security](/ja/docs/Glossary/HSTS) を使用しない場合、アクティブな MitM の対象となるユーザー (おそらくオープンな WiFi ネットワークに接続されている) は、存在しないサブドメインからの Set-Cookie ヘッダーを持つレスポンスを提供される可能性があります。ブラウザーは違法なクッキーを保存し、example.com の下の他のすべてのページにそれを送信します。

セッションの固定化は主に、ユーザーが認証するときに (たとえクッキーがすでに存在していても) セッションクッキーの値を再生成し、 CSRF トークンをユーザーに結びつけることによって緩和されるべきです。

### セッションのサイドジャッキング

### ブラウザーハイジャックマルウェア