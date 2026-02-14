# API仕様書

---

## 1. 外部API

### 1.1 Google Calendar Events API

**概要**: 本日のミーティングイベントを取得

**エンドポイント**: (Google Calendar Web API via network)

**認証**: Cookie/Session ベース（OAuth不使用）

**レスポンス構造**:

```json
{
  "items": [
    {
      "id": "string",
      "summary": "string",
      "start": { "dateTime": "ISO8601" },
      "end": { "dateTime": "ISO8601" },
      "conferenceData": {
        "entryPoints": [
          {
            "entryPointType": "video",
            "uri": "string",
            "label": "string"
          }
        ]
      },
      "hangoutLink": "string"
    }
  ]
}
```

---

## 2. 内部API

### 2.1 IPC通信

| チャネル | 方向 | 説明 |
|---------|------|------|
| `get-events` | Renderer → Main | カレンダーイベント取得要求 |
| `open-meeting` | Renderer → Main | 会議リンクを開く |
| `show-notification` | Main → Renderer | 通知表示要求 |

---

## 3. データモデル

### 3.1 CalendarEvent

```typescript
interface CalendarEvent {
  id: string;
  title: string;
  startTime: Date;
  endTime: Date;
  videoUrl?: string;
  videoType?: 'meet' | 'zoom' | 'teams';
}
```
