### API 명세서 0.3v

해당 API 명세서는  소통 플랫폼 REST API를 명세하고 있습니다.

Domain: http://localhost:4200

소통 플랫폼 모듈
고객과 디자이너간 소통을 위한 게시글, 댓글 관련 API입니다.

url: /api/v1/communication

#### - 소통 플랫폼 게시물 작성  
  
##### 설명

클라이언트로부터 Request Header의 Authorization 필드로 Bearer 토큰을 포함하여 제목, 내용을 입력받고 작성에 성공하면 성공처리를 합니다. 만약 작성에 실패하면 실패처리 됩니다. 인가 실패, 데이터베이스 에러, 데이터 유효성 검사 실패가 발생할 수 있습니다.

- method : **POST**  
- URL : **/**  

##### Request

###### Header

| name | description | required |
|---|:---:|:---:|
| Authorization | 인증에 사용될 Bearer 토큰 | O |

###### Request Body

| name | type | description | required |
|---|:---:|:---:|:---:|
| customerBoardTitle | String | 디자이너 게시물 제목 | O |
| customerBoardContents | String | 디자이너 게시물 내용 | O |

###### Example

```bash
curl -v -X POST "http://localhost:4200/api/v1/designer/board/" \
 -H "Authorization: Bearer {JWT}" \
 -d "customerBoardTitle={customerBoardTitle}" \
 -d "customerBoardContents={customerBoardContents}
```

##### Response

###### Header

| name | description | required |
|---|:---:|:---:|
| Content-Type | 반환하는 Response Body의 Content Type (application/json) | O |

###### Response Body

| name | type | description | required |
|---|:---:|:---:|:---:|
| code | String | 결과 코드 | O |
| message | String | 결과 메세지 | O |

###### Example

**응답 성공**
```bash
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
{
  "code": "SU",
  "message": "Success.",
}
```

**응답 : 실패 (데이터 유효성 검사 실패)**
```bash
HTTP/1.1 400 Bad Request
Content-Type: application/json;charset=UTF-8
{
  "code": "VF",
  "message": "Validation Failed."
}
```

**응답 : 실패 (인가 실패)**
```bash
HTTP/1.1 403 Forbidden
Content-Type: application/json;charset=UTF-8
{
  "code": "AF",
  "message": "Authorization Failed."
}
```

**응답 : 실패 (인증 실패)**
```bash
HTTP/1.1 401 Unauthorized
Content-Type: application/json;charset=UTF-8
{
  "code": "AF",
  "message": "Authentication Failed."
}
```

**응답 : 실패 (데이터베이스 오류)**
```bash
HTTP/1.1 500 Internal Server Error
Content-Type: application/json;charset=UTF-8
{
  "code": "DBE",
  "message": "Database Error."
}
```

***

게시글 수정
설명
고객이 자신이 작성한 게시글을 수정합니다. 성공 시 수정된 게시글 정보를 반환합니다.

method: PUT
url: /:postId

Request

Header
| name         | description | required |
|--------------|:-----------:|:--------:|
| Authorization | Bearer 토큰  |    O     |

Request Parameter
| name   |  type  | description | required |
|--------|:------:|:-----------:|:--------:|
| postId |  Long  |   게시글 ID   |    O     |

Request Body
| name     |  type   | description | required |
|----------|:-------:|:-----------:|:--------:|
| title    | String  |   게시글 제목   |    O     |
| content  | String  |   게시글 내용   |    O     |
| fileUrls | String[] | 첨부 파일 URL 목록 |  X  |

Response
Header
| name         | description        | required |
|--------------|--------------------|:--------:|
| Content-Type | application/json   |    O     |

Response Body
| name    |  type  |   description   | required |
|---------|:------:|:---------------:|:--------:|
| code    | String |     응답 코드     |    O     |
| message | String |     응답 메시지    |    O     |
| post    |  JSON  | 수정된 게시글 정보 |    O     |

post 상세 정보
| name      |   type   |    description    | required |
|-----------|:--------:|:-----------------:|:--------:|
| id        |   Long   |      게시글 ID      |    O     |
| title     |  String  |      게시글 제목      |    O     |
| content   |  String  |      게시글 내용      |    O     |
| isSecret  | Boolean  |      비밀글 여부      |    O     |
| createdAt |  String  |      작성 일시       |    O     |
| updatedAt |  String  |      수정 일시       |    O     |
| fileUrls  | String[] | 첨부 파일 URL 목록 |    X     |

Example
json
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
{
  "code": "SP",
  "message": "Success",
  "post": {
    "id": 42,
    "title": "긴 머리 스타일링 질문이요 (수정됨)",
    "content": "안녕하세요 디자이너님, 추가로 궁금한 게 생겨서 글 수정합니다.",
    "isSecret": false,
    "createdAt": "2023-06-02T10:30:00",
    "updatedAt": "2023-06-02T11:15:00",
    "fileUrls": [
      "https://example.com/files/longhair1.jpg",
      "https://example.com/files/longhair2.jpg", 
      "https://example.com/files/longhair3.jpg"
    ]
  }
}

게시글 삭제
설명
고객이 자신이 작성한 게시글을 삭제합니다. 성공 시 삭제된 게시글 ID를 반환합니다.

method: DELETE
url: /:postId

Request
Header
| name         | description | required |
|--------------|:-----------:|:--------:|
| Authorization | Bearer 토큰  |    O     |

Request Parameter
| name   |  type  | description | required |
|--------|:------:|:-----------:|:--------:|
| postId |  Long  |   게시글 ID   |    O     |

Response
Header
| name         | description        | required |
|--------------|--------------------|:--------:|
| Content-Type | application/json   |    O     |

Response Body
| name    |  type  | description | required |
|---------|:------:|:-----------:|:--------:|
| code    | String |   응답 코드    |    O     |
| message | String |   응답 메시지   |    O     |  
| postId  |  Long  |  삭제된 게시글 ID  |  O   |

Example
json
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
{
  "code": "SP", 
  "message": "Success",
  "postId": 42
}

게시글 신고
설명
고객 또는 디자이너가 부적절한 게시글을 신고합니다. 성공 시 신고 접수 메시지를 반환합니다.

method: POST
url: /report

Request
Header
| name         | description | required |  
|--------------|:-----------:|:--------:|
| Authorization | Bearer 토큰  |    O     |

Request Body
| name   |  type  | description | required |
|--------|:------:|:-----------:|:--------:|
| postId |  Long  |   게시글 ID   |    O     |
| reason | String |   신고 사유    |    O     |

Response
Header
| name         | description        | required |
|--------------|--------------------|:--------:|
| Content-Type | application/json   |    O     |

Response Body
| name    |  type  | description | required |
|---------|:------:|:-----------:|:--------:|
| code    | String |   응답 코드    |    O     |
| message | String |   응답 메시지   |    O     |

Example
json
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8  
{
  "code": "SR",
  "message": "Success"
}

댓글 작성

설명
고객 또는 디자이너가 특정 게시글에 댓글을 작성합니다. 성공 시 작성된 댓글 정보를 반환합니다.

method: POST
url: /:postId/comment

Request
Header
| name         | description | required |
|--------------|:-----------:|:--------:| 
| Authorization | Bearer 토큰  |    O     |

Request Parameter
| name   |  type  | description | required |  
|--------|:------:|:-----------:|:--------:|
| postId |  Long  |   게시글 ID   |    O     |

Request Body
| name     |  type  | description | required |
|----------|:------:|:-----------:|:--------:|
| content  | String |   댓글 내용    |    O     |
| parentId |  Long  | 부모 댓글 ID (대댓글인 경우) |  X   |

Response
Header
| name         | description        | required |
|--------------|--------------------|:--------:|
| Content-Type | application/json   |    O     |

Response Body
| name     |  type  |   description   | required |
|----------|:------:|:---------------:|:--------:|
| code     | String |     응답 코드      |    O     |
| message  | String |     응답 메시지     |    O     |
| comment  |  JSON  | 작성된 댓글 정보  |    O     |

comment 상세 정보
| name      |  type  |    description     | required |
|-----------|:------:|:------------------:|:--------:|
| id        |  Long  |       댓글 ID        |    O     |
| postId    |  Long  |       게시글 ID       |    O     | 
| content   | String |       댓글 내용        |    O     |
| writer    | String |      작성자 닉네임      |    O     |
| createdAt | String |       작성 일시        |    O     |  
| parentId  |  Long  | 부모 댓글 ID (대댓글인 경우) |    X     |

Example
json
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
{
  "code": "SC",
  "message": "Success",  
  "comment": {
    "id": 123,
    "postId": 42,
    "content": "좋은 질문이네요! 기장에 따라 다양한 스타일링이 가능합니다.",
    "writer": "designer_kim",  
    "createdAt": "2023-06-02T11:00:00",
    "parentId": null
  }
}

댓글 수정
설명
고객 또는 디자이너가 자신이 작성한 댓글을 수정합니다. 성공 시 수정된 댓글 정보를 반환합니다.

method: PUT
url: /:postId/comment/:commentId

Request
Header
| name         | description | required |
|--------------|:-----------:|:--------:|
| Authorization | Bearer 토큰  |    O     |

Request Parameter
| name      |  type  | description | required |
|-----------|:------:|:-----------:|:--------:|
| postId    |  Long  |   게시글 ID   |    O     |  
| commentId |  Long  |    댓글 ID    |    O     |

Request Body
| name    |  type  | description | required |   
|---------|:------:|:-----------:|:--------:|
| content | String |   댓글 내용    |    O     |

Response

Header
| name         | description        | required |
|--------------|--------------------|:--------:|
| Content-Type | application/json   |    O     |

Response Body
| name     |  type  |   description   | required | 
|----------|:------:|:---------------:|:--------:|
| code     | String |     응답 코드      |    O     |
| message  | String |     응답 메시지     |    O     |
| comment  |  JSON  | 수정된 댓글 정보  |    O     |

comment 상세 정보
| name      |  type  |    description     | required |
|-----------|:------:|:------------------:|:--------:|  
| id        |  Long  |       댓글 ID        |    O     |
| postId    |  Long  |       게시글 ID       |    O     |
| content   | String |       댓글 내용        |    O     |  
| writer    | String |      작성자 닉네임      |    O     |
| createdAt | String |       작성 일시        |    O     |
| updatedAt | String |       수정 일시        |    O     |
| parentId  |  Long  | 부모 댓글 ID (대댓글인 경우) |    X     |

##### Example

```json
HTTP/1.1 200 OK 
Content-Type: application/json;charset=UTF-8
{  
  "code": "SC",
  "message": "Success",
  "comment": { 
    "id": 123,
    "postId": 42, 
    "content": "좋은 질문이네요! 기장에 따라 다양한 스타일링이 가능합니다. 추가로 고객님의 얼굴형에 어울리는 스타일을 추천드릴게요.",  
    "writer": "designer_kim",
    "createdAt": "2023-06-02T11:00:00",
    "updatedAt": "2023-06-02T11:30:00", 
    "parentId": null
  }
}

댓글 삭제
설명
고객 또는 디자이너가 자신이 작성한 댓글을 삭제합니다. 성공 시 삭제된 댓글 ID를 반환합니다.

method: DELETE
url: /:postId/comment/:commentId

Request

Header
| name         | description | required |
|--------------|:-----------:|:--------:|
| Authorization | Bearer 토큰  |    O     |

Request Parameter
| name      |  type  | description | required |
|-----------|:------:|:-----------:|:--------:|
| postId    |  Long  |   게시글 ID   |    O     |
| commentId |  Long  |    댓글 ID    |    O     |

Response

Header
| name         | description        | required |
|--------------|--------------------|:--------:|
| Content-Type | application/json   |    O     |

Response Body
| name      |  type  | description | required |
|-----------|:------:|:-----------:|:--------:|
| code      | String |   응답 코드    |    O     |
| message   | String |   응답 메시지   |    O     |
| commentId |  Long  |  삭제된 댓글 ID  |    O     |

Example
json 
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
{
  "code": "SC",
  "message": "Success",
  "commentId": 123
}

댓글 신고
설명
고객 또는 디자이너가 부적절한 댓글을 신고합니다. 성공 시 신고 접수 메시지를 반환합니다.

method: POST
url: /comment/report

Request

Header
| name         | description | required |
|--------------|:-----------:|:--------:|
| Authorization | Bearer 토큰  |    O     |

Request Body
| name      |  type  | description | required |
|-----------|:------:|:-----------:|:--------:|
| commentId |  Long  |    댓글 ID    |    O     |
| reason    | String |   신고 사유    |    O     |

Response

Header
| name         | description        | required |
|--------------|--------------------|:--------:|
| Content-Type | application/json   |    O     |
Response Body
| name    |  type  | description | required |
|---------|:------:|:-----------:|:--------:|
| code    | String |   응답 코드    |    O     |
| message | String |   응답 메시지   |    O     |
Example
json
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
{
  "code": "SR",
  "message": "Success"
}

### 이동 요청

고객이 댓글란에서 디자이너가 올린 타 사이트 예약 링크를 클릭하여 해당 사이트로 이동할 수 있는 기능에 대한 API 명세입니다.

- method: **POST**
- url: **/comment/redirect**

#### Request

##### Header
| name         | description | required |
|--------------|:-----------:|:--------:|
| Authorization | Bearer 토큰  |    O     |

##### Request Body
| name         |  type  | description      | required |
|--------------|:------:|:----------------:|:--------:|
| commentId    |  Long  | 댓글 ID           |    O     |
| designerId   |  Long  | 디자이너 ID        |    O     |
| reservationLink | String | 이동할 예약 링크 |    O     |

#### Response

##### Header
| name         | description        | required |
|--------------|--------------------|:--------:|
| Content-Type | application/json   |    O     |

##### Response Body
| name    |  type  | description | required |
|---------|:------:|:-----------:|:--------:|
| code    | String |   응답 코드    |    O     |
| message | String |   응답 메시지   |    O     |

#### Example

```json
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
{
  "code": "SR",
  "message": "Success"
}




