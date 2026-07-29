# GASコードの差し替え（お名前を保存するため）

お名前（ニックネーム）を追加したので、GAS側も1行だけ変更が必要です。

## 手順（2分）

1. Apps Script（「アンケート受信」）を開く
2. コードを**全部消して**、下記に差し替え
3. 💾保存
4. 「デプロイ」→「**デプロイを管理**」→ ✏️編集 → バージョン「**新バージョン**」→「デプロイ」
   ※ URLは変わりません。そのまま使えます。

```javascript
function doPost(e) {
  try {
    var sheet = SpreadsheetApp.getActiveSpreadsheet().getSheets()[0];
    var d = JSON.parse(e.postData.contents);
    sheet.appendRow([
      new Date(),
      d.name || '',
      d.q1 || '', d.q2 || '', d.q3 || '', d.q4 || '', d.q5 || '',
      d.score || '', d.type || '', d.ref || ''
    ]);
    return ContentService.createTextOutput(JSON.stringify({result:'ok'}))
      .setMimeType(ContentService.MimeType.JSON);
  } catch (err) {
    return ContentService.createTextOutput(JSON.stringify({result:'error', message: String(err)}))
      .setMimeType(ContentService.MimeType.JSON);
  }
}
```

## シートの見出しも変更

スプレッドシートの1行目を、下記の順に直してください（B列に「お名前」を挿入）：

| A | B | C | D | E | F | G | H | I | J |
|---|---|---|---|---|---|---|---|---|---|
| 回答日時 | **お名前** | Q1_何歳までの見通し | Q2_収入ゼロで何ヶ月 | Q3_投資の目的 | Q4_保険の根拠 | Q5_定期的な見直し | 合計スコア | タイプ | 流入元 |

※ B列を1列挿入して「お名前」と入力するのが簡単です。
※ 既存のテスト行4件は、Voicy告知前に削除してください。
