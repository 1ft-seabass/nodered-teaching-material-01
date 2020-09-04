Node-RED で public-apis の柴犬画像 API につないで表示させるメモです。

<a href="https://github.com/public-apis/public-apis" target="_blank">public-apis</a>が、とても好きで <a href="https://protoout.studio/posts/public-apis-api-get#Animals" target="_blank">すぐにAPIを体験！public-apis 100以上のJavaScript axiosサンプル集</a> といった記事も書いています。

今回は、その知見を活かして Node-RED で public-apis の柴犬画像 API <a href="https://shibe.online/" target="_blank">shibe.online</a> につないでみます。

## Node-RED のフローの流れ

<img width="808" alt="dd0b5aee9d790b53940d3ec60b31b6d5.gif (1.1 MB)" src="https://img.esa.io/uploads/production/attachments/3062/2020/09/04/8131/3a55a0b7-939e-4d79-af0d-66955da87a9d.gif">

このようにフローをつなぎます。

### なにもないワークスペースからスタート

<img width="1227" alt="image.png (68.6 kB)" src="https://img.esa.io/uploads/production/attachments/3062/2020/09/04/8131/2bc53715-931c-4d31-aa5e-2341545ebca1.png">

まず、なにもないワークスペースからスタートしましょう。

### inject ノード

inject ノードをパレットからワークスペースに配置します。

<img width="623.25" alt="2020-09-04_22h19_38.png (29.0 kB)" src="https://img.esa.io/uploads/production/attachments/3062/2020/09/04/8131/2551305e-7bb3-41b7-8de2-ce99d89e87f8.png">

パレットから inject ノード をドラッグアンドドロップします。

### http request ノードを配置

<img width="789" alt="image.png (51.2 kB)" src="https://img.esa.io/uploads/production/attachments/3062/2020/09/04/8131/3e8f7eb0-0d7f-4c39-b8ab-85fbfd509bab.png">

http request ノードをパレットからワークスペースに配置します。

### http request ノードと inject ノードをつなぎます

<img width="687" alt="image.png (27.8 kB)" src="https://img.esa.io/uploads/production/attachments/3062/2020/09/04/8131/b2b266fa-3e57-4da6-9af7-fb0ef0c72549.png">

http request ノードと inject ノードのフローをつなぎます。

これで inject ノードがクリックされると、それをきっかけに以降のフローが動いていきます。

### http request ノードの設定

<img width="175" alt="image.png (2.1 kB)" src="https://img.esa.io/uploads/production/attachments/3062/2020/08/25/8131/f71b05ca-6336-47b2-857c-96200c41d929.png">

こちらの http request ノードで <a href="https://shibe.online/" target="_blank">shibe.online</a> につなぐ設定をします。

http request ノードをダブルクリックして詳細を設定します。

<img width="869" alt="image.png (34.6 kB)" src="https://img.esa.io/uploads/production/attachments/3062/2020/08/25/8131/e9d6f033-3fe4-4b0c-96cb-4ecf91105d39.png">

<a href="https://shibe.online/" target="_blank">shibe.online</a> の仕様は、GETリクエストでURL `http://shibe.online/api/shibes?count=3&urls=true&httpsUrls=true` とつなぎに行くと、柴犬画像が取得できますので、それに合わせて設定します。

* メソッド
    * GET
* URL
    * `http://shibe.online/api/shibes?count=3&urls=true&httpsUrls=true`
* 名前
    * shibe.online

と設定してつながるように設定します。

### JSON ノードの設定

http request ノードでデータを取得しただけだとデータが文字列のままです。JSONノードでJSONに変換して扱いやすくします。

<img width="948" alt="image.png (54.2 kB)" src="https://img.esa.io/uploads/production/attachments/3062/2020/09/04/8131/87e64a68-7544-4665-a67b-683f624f37a0.png">

JSONノードをパレットからワークスペースに配置します。

<img width="682.5" alt="2020-09-04_22h27_27.png (16.6 kB)" src="https://img.esa.io/uploads/production/attachments/3062/2020/09/04/8131/c1fca504-5fb0-4609-ac14-4227572fc33c.png">

http request ノードと JSON ノードのフローをつなぎます。

### change ノードの設定

change ノードを使って、JSONデータから画像URLを取り出します。

```js
[
"https://cdn.shibe.online/shibes/e4feffcdc53a9b42bc6b5c2c9bf88560123596ea.jpg",
"https://cdn.shibe.online/shibes/408aae17438e5b9b45c986bd46a6104e93a6e4de.jpg",
"https://cdn.shibe.online/shibes/04877898b178c9de253e21b07216949b48998aab.jpg"
]
```

受け取ったデータは、たとえば、このようになっています。配列になっているので 0 番目（一番先頭）の画像パスを拾うようにしてみます。

<img width="925" alt="image.png (43.4 kB)" src="https://img.esa.io/uploads/production/attachments/3062/2020/09/04/8131/ffef042e-6642-4035-a189-b10189c2aa57.png">

change ノードをパレットからワークスペースに配置します。

<img width="738" alt="2020-09-04_22h31_17.png (26.9 kB)" src="https://img.esa.io/uploads/production/attachments/3062/2020/09/04/8131/b6054355-7f74-4709-9da2-85cd1c9565ca.png">

JSON ノードと change ノードのフローをつなぎます。

こちらの change ノードをダブルクリックして詳細を設定します。

<img width="550" alt="image.png (19.9 kB)" src="https://img.esa.io/uploads/production/attachments/3062/2020/08/25/8131/71d8ca4b-cd2e-4356-a77e-11c283363fff.png">

* 名前
    * 0番目の画像URLを取り出す
* ルール
    * 値の代入
    * msg.payload
    * 対象の値
        * msg.payload.0

と画像のように設置します。

http request で取得したデータを JSON ノードで JSON 化して change ノードに流れ込んできた `msg.payload` データの 0 番目のデータを示す `msg.payload.0` を、`msg.payload` に値の代入をすることで0番目の画像URLを取り出すことができます。

<img width="817" alt="image.png (22.7 kB)" src="https://img.esa.io/uploads/production/attachments/3062/2020/09/04/8131/8c11c076-0086-499f-a10d-0e7e689c5600.png">

設定すると名前が設定されたため change ノードのタイトルが変わります。

### 画像を表示する node-red-contrib-image-output ノードを追加する

ここまでで、柴犬画像の画像URLを 1つ表示することができました。

画像を表示する node-red-contrib-image-output ノードをインストールします。

<img width="254" alt="image.png (9.1 kB)" src="https://img.esa.io/uploads/production/attachments/3062/2020/09/04/8131/eeaa68f6-650b-44da-9680-b5105a68fb3b.png">

右のメニューから `メニュー` ＞ `パレットの管理` と移動します。

<img width="747" alt="image.png (19.3 kB)" src="https://img.esa.io/uploads/production/attachments/3062/2020/09/04/8131/de64e827-6b59-4c33-b9cf-dcd1ba84628f.png">

ノードの追加タブをクリックしてノード追加画面に移動します。

<img width="730" alt="image.png (24.6 kB)" src="https://img.esa.io/uploads/production/attachments/3062/2020/09/04/8131/5e38074f-8518-4ced-abf7-a748d8809442.png">

ノードを検索するテキストエリアに `node-red-contrib-image-output` を入力してノードを検索して `ノードを追加` ボタンをクリックします。

<img width="916" alt="image.png (41.1 kB)" src="https://img.esa.io/uploads/production/attachments/3062/2020/09/04/8131/02a0602d-1ff5-4a22-a2f0-b9e6c0c3a248.png">

`追加` ボタンをクリックします。

<img width="911" alt="image.png (33.5 kB)" src="https://img.esa.io/uploads/production/attachments/3062/2020/09/04/8131/2001513a-bb61-4531-a298-b75ff0c83209.png">

インストールされました。

### node-red-contrib-image-output ノードをつなげる

<img width="1081" alt="image.png (83.2 kB)" src="https://img.esa.io/uploads/production/attachments/3062/2020/09/04/8131/90c8eb24-1f5d-4bf5-bc7a-ced8f295c64f.png">

node-red-contrib-image-output ノードをパレットからワークスペースに配置します。

<img width="784" alt="image.png (26.0 kB)" src="https://img.esa.io/uploads/production/attachments/3062/2020/09/04/8131/a7418a51-6b6e-4356-ac93-fba24708d8c9.png">

change ノードと node-red-contrib-image-output  ノードのフローをつなぎます。

### デプロイしてフローを保存する

これで設定は完了です。デプロイしてフローを保存します。

<img width="317" alt="image.png (17.7 kB)" src="https://img.esa.io/uploads/production/attachments/3062/2020/09/04/8131/a568b11b-34f7-4eee-837d-e7b73fbccff8.png">

デプロイボタンをクリックします。

<img width="314" alt="image.png (5.0 kB)" src="https://img.esa.io/uploads/production/attachments/3062/2020/09/04/8131/4163b83a-fe3e-451f-a11f-e6669b9bc811.png">

保存され、このように灰色にボタンの色が変わったら完了です。

## 動かしてみます

<img width="808" alt="dd0b5aee9d790b53940d3ec60b31b6d5.gif (1.1 MB)" src="https://img.esa.io/uploads/production/attachments/3062/2020/09/04/8131/3a55a0b7-939e-4d79-af0d-66955da87a9d.gif">

inject ノードを押してみましょう。http request ノードで柴犬APIからデータが読まれて JSON ノードで JSON 化されたデータが、 change ノードによって 0 番目の画像URLが取り出されて、その画像URLが node-red-contrib-image-output  ノードによって画像が表示される流れが体験できると思います。

## フローのJSONデータ

Node-REDでインポートしてすぐ使えるフローはこちらです。

```js
[{"id":"605eef04.d0868","type":"inject","z":"47eb929c.06468c","name":"","props":[{"p":"payload"},{"p":"topic","vt":"str"}],"repeat":"","crontab":"","once":false,"onceDelay":0.1,"topic":"","payload":"","payloadType":"date","x":140,"y":460,"wires":[["aaa73249.dd7a7"]]},{"id":"b0ca9f77.c1433","type":"json","z":"47eb929c.06468c","name":"","property":"payload","action":"","pretty":false,"x":650,"y":460,"wires":[["23d4c36b.95a63c"]]},{"id":"23d4c36b.95a63c","type":"change","z":"47eb929c.06468c","name":"0番目の画像URLを取り出す","rules":[{"t":"set","p":"payload","pt":"msg","to":"payload.0","tot":"msg"}],"action":"","property":"","from":"","to":"","reg":false,"x":200,"y":560,"wires":[["9b1d09a3.5b3548"]]},{"id":"aaa73249.dd7a7","type":"http request","z":"47eb929c.06468c","name":"shibe.online","method":"GET","ret":"txt","paytoqs":"ignore","url":"http://shibe.online/api/shibes?count=3&urls=true&httpsUrls=true","tls":"","persist":false,"proxy":"","authType":"","x":390,"y":460,"wires":[["b0ca9f77.c1433"]]},{"id":"9b1d09a3.5b3548","type":"image","z":"47eb929c.06468c","name":"","width":160,"data":"payload","dataType":"msg","thumbnail":false,"active":true,"pass":false,"outputs":0,"x":530,"y":560,"wires":[]}]
```

インポートする場合は <a href="https://nodered.jp/docs/user-guide/editor/workspace/import-export" target="_blank">こちら</a> を参考にしてみてください。