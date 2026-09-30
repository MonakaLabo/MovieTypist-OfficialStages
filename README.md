# MovieTypist-OfficialStages

[MovieTypist](https://github.com/MonakaLabo/MovieTypist) の公式ステージ配信用リポジトリです。ゲーム内の「公式ステージショップ」は、ここの `index.json` を参照します。

> [!NOTE]
> 公式ステージは動画を同梱していません。動画は YouTube の埋め込み再生で流れます。

## 構成

| 場所 | 内容 |
| --- | --- |
| `index.json` | 公式ステージ一覧 |
| [Releases の `stages`](../../releases/tag/stages) | 各ステージの `.mtt` とサムネイル（`<id>-v<版>.mtt` / `.png`） |

## `index.json`

```json
{
  "formatVersion": 1,
  "updatedAt": "2026-10-01T00:00:00Z",
  "stages": [
    {
      "id": "…",                 // ステージの固有 ID（metadata.json の id と同じ）
      "version": 2,               // 版。更新のたびに 1 増える
      "title": "…",
      "author": "…",
      "comment": "…",
      "url": "https://www.youtube.com/watch?v=…",
      "addedAt": "2026-10-01",    // 初めて公開した日
      "updatedAt": "2026-10-12",  // 最新版を公開した日
      "note": "…",                // この版の更新メモ
      "speedAvg": 7.08,           // 必要打鍵速度の平均 [打/秒]
      "speedMax": 12.4,           // 必要打鍵速度の最大 [打/秒]
      "lines": 44,                // 入力のあるライン数
      "duration": 205.0,          // 動画の長さ [秒]
      "download": "https://github.com/…/releases/download/stages/<id>-v2.mtt",
      "thumbnail": "https://github.com/…/releases/download/stages/<id>-v2.png",
      "size": 12345,
      "sha256": "…",              // 最新版 .mtt の SHA-256
      "previousSha256": ["…"]     // 旧版 .mtt の SHA-256
    }
  ]
}
```

## 公式ステージの判定

`.mtt` の `metadata.json` には `official` があります。ゲームは `official: true` のステージのファイルの SHA-256 を、取得済み（キャッシュ）の `index.json` の `sha256` / `previousSha256` と照合し、一致したものだけを公式ステージとして表示します。一覧を一度も取得していない場合は `official` の値をそのまま使います。

## 公式ステージの公開・更新（管理者向け）

MovieTypist リポジトリの隣にこのリポジトリをクローンした状態で、MovieTypist 側から実行します。

```
powershell -ExecutionPolicy Bypass -File tools\official\publish-stage.ps1 -Stage path\to\stage.mtt -Note "更新メモ"
```

公式ステージ用の `.mtt` の書き出し（`official: true`・版・SPEED avg./max. の設定）、`index.json` の更新、Releases へのアップロード、push までを行います。
