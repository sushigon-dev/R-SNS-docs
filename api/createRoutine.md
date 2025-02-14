# sushigon-backend API 設計書

- RESTful API にする

## 習慣の投稿

- CreateRoutine
- endpoint: `/api/routine`
- method: `POST`

### 成功

- HTTP/1.1 201 Created
  - tags は空でも可
  ```bash
  curl -X POST \
    -H "Content-Type: application/json" \
    -d '{
          "title": "１日１ページ本を読む！",
          "tags": ["読書", "毎日"],
          "body": "１日１ページ何かしらの本を読む、絵本でもいい",
          "user": {
            "id": "3e93f266-6566-4b7d-a4a8-e3cb2a7bfc50",
            "name": "与謝蕪村"
          }
        }' \
     http://localhost:8080/api/routine
  ```
  ```json
  {
    "id": "0effc6e3-f4b4-4a1b-a003-d99322a892e3"
    "createdAt": "2025021311001102",
    "updatedAt": "2025021311001102",
  }
  ```

### 失敗

- HTTP/1.1 400 Bad Request
  - 渡される情報が不正（フィールドが足りない）
  - 例: title と body がない
  ```bash
  curl -X POST \
    -H "Content-Type: application/json" \
    -d '{
          "tags": ["読書", "毎日"],
          "user": {
            "id": "3e93f266-6566-4b7d-a4a8-e3cb2a7bfc50",
            "name": "与謝蕪村"
          }
        }' \
     http://localhost:8080/api/routine
  ```
  ```json
  {
    "error": "不正な入力: Key: 'CreateRoutineInput.Title' Error:Field validation for 'Title' failed on the 'required' tag\nKey: 'CreateRoutineInput.Body' Error:Field validation for 'Body' failed on the 'required' tag"
  }
  ```
- HTTP/1.1 400 Bad Request
  - 渡される情報が不正（値が不正）
  ```bash
  curl -X POST \
    -H "Content-Type: application/json" \
    -d '{
          "title": "１日１ページ本を読む！あsdふぁsdふぁえふうぇあげwがうぇg",
          "tags": ["読書", "毎日"],
          "body": "１日１ページ何かしらの本を読む、絵本でもいい",
          "user": {
            "id": "3e93f266-6566-4b7d-a4a8-e3cb2a7bfc50",
            "name": "与謝蕪村"
          }
        }' \
     http://localhost:8080/api/routine
  ```
  ```json
  { "error": "タイトルは1文字以上20文字以下" }
  ```
- HTTP/1.1 500 Internal Server Error
  - データの取得中の予期しないエラー
