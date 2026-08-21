# 敬愛大学 オープンキャンパス｜キャンパス・フォトブース

スマホのカメラで撮影 → 敬愛大学のキャンパス写真を背景に合成 → 保存・シェア。
サーバー不要、GitHub Pages にそのまま置けます。画像処理はすべて端末内（外部送信なし）。

## ファイル構成

```
index.html          ← これ1枚で全機能
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

**`images/` は今プレースホルダーです。** お手元の8枚（shinkoikuto5/6/7/9/10, shikyoikuto3/8, soshikizu202404c）を
上の対応表どおりのファイル名にリネームして上書きしてください。横長・1600px幅程度が最適です。

対応を変えたい場合は `index.html` 内の `BACKGROUNDS` 配列を編集します。

## GitHub Pages への公開

1. GitHub で新規リポジトリを作成（例：`keiai-opencampus`）、Public にする
2. `index.html` と `images/` フォルダをアップロード（Add file → Upload files）
3. Settings → Pages → Source を **Deploy from a branch**、Branch を **main / (root)** にして Save
4. 1〜2分後に `https://<ユーザー名>.github.io/keiai-opencampus/` で公開

コマンド派なら:

```bash
git init && git add . && git commit -m "open campus photo booth"
git branch -M main
git remote add origin https://github.com/<ユーザー名>/keiai-opencampus.git
git push -u origin main
```

カメラ API は HTTPS 必須ですが、GitHub Pages は標準で HTTPS なのでそのまま動きます。
ローカル確認は `python3 -m http.server` → `http://localhost:8000`（localhost は例外的に許可されます）。

## 機能

- 内カメラ／外カメラ切替、3秒カウントダウン、ルーバー（新校舎の波形ファサード）を模したシャッター演出
- レイアウト3種：丸窓／上下2分割／全面＋フレーム
- トーン3種：ナチュラル／ハイキー／フィルム
- 一言キャプション（最大24文字）と日付スタンプの自動焼き込み
- 出力 1080×1350（Instagram 4:5）、JPEG保存＋Web Share対応端末では直接シェア
- カメラが開けない端末向けにファイル選択フォールバック

## 当日運用のヒント

- 受付でこのURLのQRコードを配布。iPhone/Android どちらの標準ブラウザでも動きます
- タブレットを三脚に固定して据置ブースにする場合は「外カメラ」に切替えて使用
- ハッシュタグは `caption` 欄の初期値（`#敬愛大生になる日`）を書き換えれば全端末に反映されます
