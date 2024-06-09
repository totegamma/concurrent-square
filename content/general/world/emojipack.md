+++
title="絵文字パック"
weight=1
+++

## 絵文字パック
Concurrentでは、各Concurrentサーバーの管理者へ申請することなく、ユーザーの操作のみでカスタム絵文字を追加することが可能です。  
任意の絵文字をConcurrentに追加するには、"絵文字パック"を作成する必要があります。

### 絵文字パックの作成方法
絵文字パックの作成には、下記のものが必要です。
1. 絵文字の実体となる画像ファイル
2. 絵文字の画像ファイルを配信するWebサーバーまたはサービス
3. 絵文字パックを定義するJSONファイル
4. 絵文字パックを定義するJSONファイルを配信するWebサーバーまたはサービス

#### 1. 絵文字の実体となる画像ファイルの準備
まず、絵文字の実体となる画像ファイルを用意します。
絵文字として使用できる画像形式は

- PNG
- APNG
- GIF
- JPEG

です。  
また、投稿が他のActivityPub対応SNSに共有された際、正常に表示するためには、追加で下記の要件を満たす必要があります。

- 画像の縦幅が256px以下であること
- 容量が50KB以下であること

さらに、Mastodonでも正常に表示されるようにする場合は、絵文字を1:1の正方形にする必要があります。  
これは、Mastodonが横長の絵文字を正常に表示できないためです。  
なお、Concurrent、Misskeyは横長の絵文字にも対応しています。

#### 2. 絵文字をWebサーバーに配置する
絵文字用の画像ファイルを作成したら、インターネット上のどこからでも絵文字画像にアクセスできるように、画像をWebサーバーに公開し、URLを入力するだけで画像を表示できるようにする必要があります。  
URLの規格に準拠しており、かつ`https://`から始まるURLであれば、どのようなURLでも構いません。

例）
```
https://example.com/files/hoge.gif
```
※ 拡張子はついていなくでも大丈夫です。

但し、画像ファイルの配信時に、クロスオリジンを許可するよう、HTTPレスポンスヘッダに次の情報を付加する必要があります。

```
Access-Control-Allow-Origin "*"
```

レスポンスヘッダに任意のキーを付加する方法については、各Webサーバーのマニュアルを参照してください。

また、絵文字URLは投稿(カレント)に含まれる絵文字を表示する際に都度参照されるため、一度指定したURLは変更してはいけません。

#### 3. 絵文字パックを定義するJSONの作成
絵文字パックを定義するJSONファイル（以降、絵文字パックJSONと呼びます。）には絵文字パックの情報と、絵文字パックに含まれる絵文字の画像ファイルURL、絵文字をConcurrent上で呼び出すのに必要なショートコード(`:hoge:`のような文字列)などの情報を記載する必要があります。

JSONのフォーマットは下記のとおりです。  
※ 実際のJSONにはコメント（`//` から始まる部分）を含めないでください。  

例)
```JSON
{
  "name": "HogeEmojis", // 絵文字パック名
  "version": "1.0.0", // 絵文字パックのバージョン
  "description": "ゆかいな絵文字パック", // 絵文字パックの説明
  "credit": "2023 HogeEmojis", // 絵文字の著作権やライセンスの情報など 
  "iconURL": "https://example.com/files/emojipack.png", // 絵文字パックのアイコン
  "emojis": [
    { // 1つ目の絵文字
      "shortcode": "hoge", // 絵文字のショートコード(`:`を除いた部分)
      "imageURL": "https://example.com/files/hoge.gif", // 絵文字の実体となる画像ファイル
      "aliases": ["ほげ", "ホゲ"] // 絵文字のエイリアス(別名: 絵文字を検索する際のキーワード)
    },
    { // 2つ目の絵文字
      "shortcode": "huga", // 絵文字のショートコード(`:`を除いた部分)
      "imageURL": "https://example.com/files/huga.png", // 絵文字の実体となる画像ファイル
      "aliases": ["ふが", "フガ"] // 絵文字のエイリアス(別名: 絵文字を検索する際のキーワード)
    },
    { // 3つ目の絵文字
      "shortcode": "piyo", // 絵文字のショートコード(`:`を除いた部分)
      "imageURL": "https://example.com/files/piyo.jpg", // 絵文字の実体となる画像ファイル
      "aliases": ["ぴよ", "ピヨ", "ひよこ"] // 絵文字のエイリアス(別名: 絵文字を検索する際のキーワード)
    }
    ...省略...
  ]
}
```

1つの絵文字パックに含める絵文字の数について、仕様上の指定や制限は特にありません。  
なお、ショートコードは下記のルールにしたがって定義をする必要があります。 準拠しない場合、SNSによっては絵文字が正常に表示されない可能性があります。
- ショートコードの文字数は2文字以上であること
- アルファベット及び`_`(アンダースコア)のいずれかの文字のみを使用すること

####  4. 絵文字パックを定義するJSONの公開
画像ファイル同様、絵文字パックJSONを公開し、URLを入力するだけで絵文字パックJSONにアクセスできるようにする必要があります。  
URLの規格に準拠しており、かつ`https://`から始まるURLであれば、どのようなURLでも構いません。

例）
```
https://example.com/metas/emojipack.json
```
※ 拡張子はついていなくでも大丈夫です。

但し、絵文字パックJSONの配信時に、クロスオリジンを許可するよう、HTTPレスポンスヘッダに次の情報を付加する必要があります。

```
Access-Control-Allow-Origin "*"
```

レスポンスヘッダに任意のキーを付加する方法については、各Webサーバーのマニュアルを参照してください。

もしくは、[GitHub Gist](https://gist.github.com/)を使用して、JSONファイルを公開することもできます。