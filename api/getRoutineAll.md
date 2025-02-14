# sushigon-backend API 設計書

- RESTful API にする

## 投稿された習慣を全て取得

- getRoutineAll
- エンドポイント: `/api/routine`
- HTTP メソッド: `GET`

### 成功

- HTTP/1.1 200 OK
  リクエスト例（curl コマンド使うのが一般的）
  ```bash
  curl -X GET \
    http://localhost:8080/api/routine
  ```

  レスポンス例（json じゃなくてもいい）（適当な例）
  ```json
  {
    "id": "0effc6e3-f4b4-4a1b-a003-d99322a892e3"
    "createdAt": "2025021311001102",
    "updatedAt": "2025021311001102",
    "title": "1日1回バックエンド",
    "tags": "バックエンド",
    "body": "バックエンドおもろい",
    "user": {
	    "id": "d5b9e5db-977e-40bd-a027-376b13c93712"
	    "name": "与謝野晶子"
    }
  }
  ```

### 失敗

- HTTP/1.1 400 Bad Request
  - リクエストが間違っている
  - 例: エンドポイント間違い
  ```bash
  curl -X GET\
    http://localhost:8080/api/
  ```

- HTTP/1.1 500 Internal Server Error
  - データの取得中の予期しないエラー
  - 例:渡す情報が少ない
  ```bash
  curl -X POST \
    -H "Content-Type: application/json"\
    -d '{
	    "tags": ["バックエンド"],
	    "user": {
	      "id": "011b1d47-6e1e-4392-988b-a7af97a7f4c8",
	      "name": "与謝野晶子"
	    }
    }
	}' 
  ```