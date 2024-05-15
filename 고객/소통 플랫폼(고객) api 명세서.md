### API 명세서 0.1v

해당 API 명세서는 '헤어케어 디자이너 매칭 플랫폼 서비스'의 소통 플랫폼 REST API를 명세하고 있습니다.

Domain: http://localhost:4200

소통 플랫폼 모듈
고객과 디자이너간 소통을 위한 게시글, 댓글 관련 API입니다.

url: /api/v1/communication

게시글 작성
설명
고객이 소통 플랫폼에서 새로운 게시글을 작성합니다. 성공 시 작성된 게시글 정보를 반환합니다.

method: POST
url: /

Request

Header
| name         | description | required |
|--------------|:-----------:|:--------:|
| Authorization | Bearer 토큰  |    O     |

Request Body
| name      |  type   | description | required |
|-----------|:-------:|:-----------:|:--------:|
| title     | String  |  게시글 제목   |    O     |
| content   | String  |  게시글 내용   |    O     |
| isSecret  | Boolean |   비밀글 여부  |    O     |
| fileUrls  | String[] |  첨부 파일 URL 목록 |  X   |

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
| post    |  JSON  | 작성된 게시글 정보 |    O     |

post 상세 정보
| name      |   type   |    description    | required |
|-----------|:--------:|:-----------------:|:--------:|
| id        |   Long   |      게시글 ID      |    O     |
| title     |  String  |      게시글 제목      |    O     |
| content   |  String  |      게시글 내용      |    O     |
| isSecret  | Boolean  |      비밀글 여부      |    O     |
| createdAt |  String  |      작성 일시       |    O     |
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
    "title": "긴 머리 스타일링 질문이요",
    "content": "안녕하세요 디자이너님, 제가 머리카락이 길어서 스타일링하기 힘든데 어떻게 하면 좋을까요?",
    "isSecret": false,
    "createdAt": "2023-06-02T10:30:00",
    "fileUrls": [
      "https://example.com/files/longhair1.jpg",
      "https://example.com/files/longhair2.jpg"
    ]
  }
}

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




# API 명세서

해당 API 명세서는 '헤어어드바이저 디자이너 매칭 플랫폼 서비스'의 고객소통파트 REST API를 명세하고 있습니다.

- Domain: <http://localhost:4200>

## 소통 플랫폼 모듈

고객과 디자이너간 1:1 소통을 위한 채팅 기능 관련 API입니다.

- url: /api/v1/communication

### 채팅방 생성

#### 설명

고객이 디자이너의 댓글에서 채팅 시작 버튼을 눌러 채팅방을 생성합니다. 성공 시 채팅방 정보를 반환합니다.

- method: **POST**
- url: **/**

#### Request

##### Header
| name         | description | required |
|--------------|:-----------:|:--------:|
| Authorization | Bearer 토큰  |    O     |
  
##### Request Body
| name       |  type  |   description   | required |
|------------|:------:|:---------------:|:--------:|
| designerId |  Long  | 디자이너 사용자 ID |    O     |
  
#### Response

##### Header
| name         | description        | required |
|--------------|--------------------|:--------:|
| Content-Type | application/json   |    O     |
  
##### Response Body
| name     |  type  |   description   | required |
|----------|:------:|:---------------:|:--------:|
| code     | String |     응답 코드     |    O     |
| message  | String |     응답 메시지    |    O     |
| chatRoom |  JSON  | 생성된 채팅방 정보 |    O     |
  
##### chatRoom 상세 정보
| name      |  type  |   description   | required |
|-----------|:------:|:---------------:|:--------:|
| id        |  Long  |    채팅방 ID     |    O     |
| userId    |  Long  |   고객 사용자 ID   |    O     |
| designerId |  Long  |  디자이너 사용자 ID  |    O     |
| createdAt | String |    생성 일시     |    O     |
##### Example

```json
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
{
  "code": "SC",
  "message": "Success",
  "chatRoom": {
    "id": 23,
    "userId": 42,
    "designerId": 87,
    "createdAt": "2023-06-01T15:30:00"
  }
}

채팅 메시지 전송
설명
특정 채팅방에서 고객 또는 디자이너가 메시지를 전송합니다. 성공 시 전송된 메시지 정보를 반환합니다.

method: POST
url: /:roomId/message

Request

Header
| name         | description | required |
|--------------|:-----------:|:--------:|
| Authorization | Bearer 토큰  |    O     |

Request Parameter
| name   |  type  | description | required |
|--------|:------:|:-----------:|:--------:|
| roomId |  Long  |   채팅방 ID   |    O     |

Request Body
| name    |  type  | description | required |
|---------|:------:|:-----------:|:--------:|
| content | String |   메시지 내용  |    O     |

Response

Header
| name         | description        | required |
|--------------|--------------------|:--------:|
| Content-Type | application/json   |    O     |

Response Body
| name    |  type  |  description   | required |
|---------|:------:|:--------------:|:--------:|
| code    | String |    응답 코드     |    O     |
| message | String |    응답 메시지    |    O     |
| chatMessage | JSON | 전송된 메시지 정보 |    O     |

chatMessage 상세 정보
| name     |  type  |    description    | required |
|----------|:------:|:-----------------:|:--------:|
| id       |  Long  |     메시지 ID      |    O     |
| roomId   |  Long  |      채팅방 ID      |    O     |
| senderId |  Long  |    발신자 사용자 ID   |    O     |
| content  | String |      메시지 내용      |    O     |
| sentAt   | String |      전송 일시      |    O     |

Example

```json
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
{
  "code": "SS",  
  "message": "Success",
  "chatMessage": {
    "id": 123, 
    "roomId": 23,
    "senderId": 42,  
    "content": "안녕하세요 상담 가능할까요?",
    "sentAt": "2023-06-01T15:35:00" 
  }
}

채팅 메시지 목록 조회

설명

특정 채팅방의 메시지 목록을 조회합니다. 한 번에 최대 50개씩 조회되며, 이전 메시지가 더 있다면 이전 메시지 조회를 위한 nextPage 값을 내려줍니다.

method: GET
url: /:roomId/messages

Request
Header
| name         | description | required |
|--------------|:-----------:|:--------:|
| Authorization | Bearer 토큰  |    O     |

Request Parameter
| name   |  type  | description | required |
|--------|:------:|:-----------:|:--------:|
| roomId |  Long  |   채팅방 ID   |    O     |
| page   |  Int   |    페이지 번호  |    X     |

Response

Header
| name         | description        | required |  
|--------------|--------------------|:--------:|
| Content-Type | application/json   |    O     |

Response Body
| name     |   type    |   description    | required |
|----------|:---------:|:----------------:|:--------:|
| code     |  String   |     응답 코드      |    O     |
| message  |  String   |     응답 메시지     |    O     |
| messages | JSON Array |    메시지 목록     |    O     |
| nextPage |    Int    | 다음 페이지 번호 (없으면 null) |    X     |

messages 내 각 객체 상세 정보
| name     |  type  |    description    | required |
|----------|:------:|:-----------------:|:--------:|  
| id       |  Long  |     메시지 ID      |    O     |
| roomId   |  Long  |      채팅방 ID      |    O     |
| senderId |  Long  |    발신자 사용자 ID   |    O     |
| content  | String |      메시지 내용      |    O     | 
| sentAt   | String |      전송 일시      |    O     |

Example
json
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
{
  "code": "SL",
  "message": "Success",
  "messages": [
    {
      "id": 456,
      "roomId": 23, 
      "senderId": 87,
      "content": "안녕하세요 고객님, 상담 가능합니다.",
      "sentAt": "2023-06-01T15:40:00"
    },
    {
      "id": 123,
      "roomId": 23,
      "senderId": 42,
      "content": "안녕하세요 상담 가능할까요?", 
      "sentAt": "2023-06-01T15:35:00"
    }
  ],
  "nextPage": 1
}

채팅 메시지 신고

설명

특정 채팅 메시지를 신고합니다. 신고가 성공적으로 접수되면 성공 메시지를 반환합니다.

method: POST
url: /report

Request

Header
| name         | description | required |  
|--------------|:-----------:|:--------:|
| Authorization | Bearer 토큰  |    O     |

Request Body
| name      |  type  | description | required |
|-----------|:------:|:-----------:|:--------:|
| messageId |  Long  |   메시지 ID   |    O     |
| reason    | String |    신고 사유   |    O     |

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
HTTP/1.1 200 OK 
Content-Type: application/json;charset=UTF-8
{
  "code": "SR",
  "message": "Success"  
}

채팅방 나가기

설명

사용자가 특정 채팅방에서 나갑니다. 나가기가 성공하면 나간 채팅방 ID와 성공 메시지를 반환합니다.

method: DELETE
url: /:roomId

Request
Header
| name         | description | required |
|--------------|:-----------:|:--------:|  
| Authorization | Bearer 토큰  |    O     |

Request Parameter
| name   |  type  | description | required |
|--------|:------:|:-----------:|:--------:|
| roomId |  Long  |   채팅방 ID   |    O     |

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
| roomId  |  Long  |   나간 채팅방 ID   |    O     |

Example

HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
{
  "code": "SL",
  "message": "Success", 
  "roomId": 23
}