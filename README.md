# 敬愛大学 オープンキャンパス｜キャンパス・フォトブース

スマホのカメラで撮影 → ブラウザ内AIで人物を切り抜き → キャンパスを背景に合成 → イラスト化して保存・シェア。
サーバー不要。GitHub Pages にそのまま置けます。撮影画像は端末内だけで処理され、外部送信は一切ありません。

## ファイル構成

```
index.html          ← これ1枚で全機能
.nojekyll           ← GitHub Pages の Jekyll 処理を無効化（置くだけ）
images/
  campus1.jpg  アクティブラーニング教室
  campus2.jpg  グループワークルーム
  campus3.jpg  新校舎 外観
  campus4.jpg  キャンパス全景
  campus5.jpg  ラーニングコモンズ
  campus6.jpg  セミナールーム
  campus7.jpg  学生ラウンジ
  campus8.jpg  カフェテリア
```

**`images/` の中身はプレースホルダーです。** お手元の8枚に差し替えてください。

ファイル名は元のまま（`shinkoikuto10.jpg` `shikyoikuto3.jpg` `soshikizu202404c.jpg` など）でも読み込めます。
ページ側が `images/` → `img/` → 直下 の順に、`.jpg / .JPG / .jpeg / .png / .webp` を総当たりで探します。
どれも見つからない場合は「背景を選ぶ」の上に、不足しているファイル名が赤枠で表示されます。

対応を変えたいときは `index.html` 内の `BACKGROUNDS` 配列を編集してください。

## GitHub Pages への公開

1. リポジトリを作成（Public）
2. `index.html` `.nojekyll` `images/` をアップロード
   - Web UI の Upload files は**フォルダごとドラッグ&ドロップ**しないと `images/` が作られません
3. Settings → Pages → Source を **Deploy from a branch**、Branch を **main / (root)** で Save
4. 1〜2分後、Settings → Pages 上部に表示される URL で公開

```bash
git init && git add . && git commit -m "open campus photo booth"
git branch -M main
git remote add origin https://github.com/<ユーザー名>/<リポジトリ名>.git
git push -u origin main
```

カメラAPIはHTTPS必須ですが、GitHub Pages は標準でHTTPSなのでそのまま動きます。
ローカル確認は `python3 -m http.server` → `http://localhost:8000`。

## 機能

**撮影**
- 内カメラ／外カメラ切替、3秒カウントダウン
- 新校舎の波形ファサードを模したルーバー・シャッター演出
- カメラが開けない端末向けにファイル選択フォールバック

**AI処理（すべてブラウザ内で完結）**
- 人物切り抜き：MediaPipe Selfie Segmentation。初回のみモデル約2MBをCDNから取得
- スタイル変換：色の階調量子化＋Sobelフィルタによる輪郭線描画（イラスト風／アニメ風／版画風）

**仕上げ**
- レイアウト4種：AI切り抜き合成／丸窓／上下2分割／全面＋フレーム
- トーン3種：ナチュラル／ハイキー／フィルム
- 一言キャプション（24文字）＋日付＋撮影スポット名を自動で焼き込み
- 出力 1080×1350（Instagram 4:5）、JPEG保存＋Web Share対応端末では直接シェア

## 当日運用のヒント

- 受付でURLのQRコードを配布。iPhone / Android の標準ブラウザで動きます
- 会場Wi-Fiが弱いと初回のモデル取得に時間がかかります。受付で1台先に開いておくと以降は速いです
- タブレットを三脚固定して据置ブースにする場合は「外カメラ」に切替
- ハッシュタグは `index.html` の `<input id="caption" ... value="...">` を書き換えれば全端末に反映されます

## 既知の限界

顔立ちを別のキャラクターに描き起こす「生成AIアバター」は、静的サイトでは実現できません。
画像生成APIのキーをページに書くと誰でも盗めるため、Cloudflare Workers 等の中継サーバーが必須になります。
本ページのAI処理は、撮影した本人の写真を切り抜き・加工するところまでです。
