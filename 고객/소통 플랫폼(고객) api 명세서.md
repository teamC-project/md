소통 플랫폼(공통)api 명세서 0.2v
<h1 style='background-color: rgba(55, 55, 55, 0.4); text-align: center'>API 명세서</h1>
소통 플랫폼의 REST API 명세서입니다.

Domain: http://localhost:4200


<h2 style='background-color: rgba(55, 55, 55, 0.2); text-align: center'>게시물 모듈</h2>
게시물 조회, 작성, 수정, 삭제와 관련된 REST API 모듈입니다.

url: /api/v1/customer_board


### - 게시물 목록 조회

설명
게시물 목록을 조회합니다. 페이지네이션과 검색 기능을 지원합니다.

#### Method: GET
#### URL: /api/v1/customer_board

##### Request

**Query Parameters**
| Name    | Type    | Description  | Required |
|---------|---------|--------------|----------|
| page    | Integer | 페이지 번호   | X        |
| size    | Integer | 페이지 사이즈 | X        |
| title   | String  | 검색할 제목   | X        |

##### Example

```bash
curl -X GET "http://localhost:4200/api/v1/customer_board?page=1&size=10&title=검색어"

##### Response  

##### Header

| name | description | required |
|---|:---:|:---:|
| Content-Type | application/json | O |

##### Response Body

| name | type | description | required |
|---|:---:|:---:|:---:|
| totalCount | Integer | 전체 게시물 수 | O |
| currentPage | Integer | 현재 페이지 번호 | O |
| totalPages | Integer | 전체 페이지 수 | O |
| customerBoardList | Array | 게시물 목록 | O |
| customerBoardList.boardNumber | Integer | 게시물 번호 | O |
| customerBoardList.title | String | 게시물 제목 | O |
| customerBoardList.writerId | String | 작성자 아이디 | O |
| customerBoardList.writeDatetime | String | 작성일 (yy.mm.dd) | O |
| customerBoardList.viewCount | Integer | 조회수 | O |

##### Example

```json
HTTP/1.1 200 OK
Content-Type: application/json
{
  "totalCount": 100,
  "currentPage": 1,
  "totalPages": 10,
  "customerBoardList": [
    {
      "boardNumber": 1,
      "title": "게시물 제목 1",
      "writerId": "cus***",
      "writeDatetime": "21.05.01",
      "viewCount": 10
    },
    {
      "boardNumber": 2,
      "title": "게시물 제목 2",
      "writerId": "cus***",
      "writeDatetime": "21.05.02",
      "viewCount": 5
    }
  ]
}

### - 게시물 상세 조회

설명
게시물의 상세 정보를 조회합니다. 비밀글인 경우 작성자, 디자이너, 관리자만 조회 가능합니다.

#### method: GET
#### URL: /api/v1/customer_board/{boardNumber}

##### Request

##### Path Variables

| name | type | description | required |
|---|:---:|:---:|:---:|
| postId | Integer | 게시물 ID | O |

##### Example

```bash
curl -X GET "http://localhost:4200/api/v1/customer_board/1"

##### Response

##### Header

| name | description | required |
|---|:---:|:---:|
| Content-Type | application/json | O |

##### Response Body

| name | type | description | required |
|---|:---:|:---:|:---:|
| boardNumber | Integer | 게시물 번호 | O |
| title | String | 게시물 제목 | O |
| contents | String | 게시물 내용 | O |
| writerId | String | 작성자 아이디 | O |
| writeDatetime | String | 작성일 (yyyy-MM-dd HH:mm:ss) | O |
| viewCount | Integer | 조회수 | O |
| comments | Array | 댓글 목록 | O |
| comments.id | Integer | 댓글 ID | O |
| comments.writerId | String | 댓글 작성자 아이디 | O |
| comments.contents | String | 댓글 내용 | O |
| comments.writeDatetime | String | 댓글 작성일 (yyyy-MM-dd HH:mm:ss) | O |

##### Example

```json
HTTP/1.1 200 OK
Content-Type: application/json
{
  "boardNumber": 1,
  "title": "게시물 제목",
  "contents": "게시물 내용",
  "writerId": "cus123",
  "writeDatetime": "2021-05-01 10:00:00",
  "viewCount": 10,
  "comments": [
    {
      "id": 1,
      "writerId": "cus456",
      "contents": "댓글 내용",
      "writeDatetime": "2021-05-01 11:00:00"
    },
    {
      "id": 2,
      "writerId": "cus789",
      "contents": "댓글 내용",
      "writeDatetime": "2021-05-01 12:00:00"
    }
  ]
}

### - 게시물 작성

설명
새로운 게시물을 작성합니다.

#### method: POST
#### URL: /api/v1/customer_board

##### Request

##### Header

| name | description | required |
|---|:---:|:---:|
| Content-Type | application/json | O |

##### Request Body

| name | type | description | required |
|---|:---:|:---:|:---:|
| title | String | 게시물 제목 | O |
| contents | String | 게시물 내용 | O |

##### Example

```bash
curl -X POST "http://localhost:4200/api/v1/customer_board" \
-H "Content-Type: application/json" \
-d '{
  "title": "새로운 게시물 제목",
  "contents": "새로운 게시물 내용"
}'

##### Response

##### Header

| name | description | required |
|---|:---:|:---:|
| Content-Type | application/json | O |

##### Response Body

| name | type | description | required |
|---|:---:|:---:|:---:|
| boardNumber | Integer | 생성된 게시물 번호 | O |

##### Example

```json
HTTP/1.1 201 Created
Content-Type: application/json
{
  "boardNumber": 1
}

### - 게시물 수정

설명
기존 게시물을 수정합니다. 작성자만 수정 가능합니다.

#### method: PUT
#### URL: /api/v1/customer_board/{boardNumber}

##### Request

##### Path Variables

| name | type | description | required |
|---|:---:|:---:|:---:|
| postId | Integer | 게시물 ID | O |

##### Header

| name | description | required |
|---|:---:|:---:|
| Content-Type | application/json | O |

##### Request Body

| name | type | description | required |
|---|:---:|:---:|:---:|
| title | String | 수정할 게시물 제목 | O |
| contents | String | 수정할 게시물 내용 | O |

##### Example

```bash
curl -X PUT "http://localhost:4200/api/v1/customer_board/1" \
-H "Content-Type: application/json" \
-d '{
  "title": "수정된 게시물 제목",
  "contents": "수정된 게시물 내용"  
}'

##### Response

##### Header

| name | description | required |
|---|:---:|:---:|
| Content-Type | application/json | O |

##### Response Body

| name | type | description | required |
|---|:---:|:---:|:---:|
| boardNumber | Integer | 수정된 게시물 번호 | O |

##### Example

```json
HTTP/1.1 200 OK
Content-Type: application/json
{
  "boardNumber": 1
}

### - 게시물 삭제

설명
기존 게시물을 삭제합니다. 작성자만 삭제 가능합니다.

#### method: DELETE
#### URL: /api/v1/customer_board/{boardNumber}

##### Request

##### Path Variables

| name | type | description | required |
|---|:---:|:---:|:---:|
| boardNumber | Integer | 게시물 번호 | O |

##### Example

```bash
curl -X DELETE "http://localhost:4200/api/v1/customer_board/1"

##### Response

##### Header

| name | description | required |
|---|:---:|:---:|

#####Example

HTTP/1.1 204 No Content

<h2 style='background-color: rgba(55, 55, 55, 0.2); text-align: center'>댓글 모듈</h2>
댓글 작성, 수정, 삭제와 관련된 REST API 모듈입니다.

url: /api/v1/customer_board_comments


### - 댓글 작성
설명
게시물에 새로운 댓글을 작성합니다. 로그인한 사용자만 작성 가능합니다.

#### method: POST
#### URL: /api/v1/customer_board_comments

##### Request

##### Header

| name | description | required |
|---|:---:|:---:|
| Content-Type | application/json | O |

##### Request Body

| name | type | description | required |
|---|:---:|:---:|:---:|
| boardNumber | Integer | 게시물 번호 | O |
| contents | String | 댓글 내용 | O |

##### Example

```bash
curl -X POST "http://localhost:4200/api/v1/customer_board_comments" \
-H "Content-Type: application/json" \
-d '{
  "boardNumber": 1,
  "contents": "새로운 댓글 내용"
}'

##### Response

##### Header

| name | description | required |
|---|:---:|:---:|
| Content-Type | application/json | O |

##### Response Body

| name | type | description | required |
|---|:---:|:---:|:---:|
| id | Integer | 생성된 댓글 ID | O |

##### Example

```json
HTTP/1.1 201 Created
Content-Type: application/json
{
  "id": 1
}

### - 댓글 수정

설명
기존 댓글을 수정합니다. 작성자만 수정 가능합니다.

#### method: PUT
#### URL: /api/v1/customer_board_comments/{id}
##### Request

##### Path Variables

| name | type | description | required |
|---|:---:|:---:|:---:|
| id | Integer | 댓글 ID | O |

##### Header

| name | description | required |
|---|:---:|:---:|
| Content-Type | application/json | O |

##### Request Body

| name | type | description | required |
|---|:---:|:---:|:---:|
| contents | String | 수정할 댓글 내용 | O |

##### Example

```bash
curl -X PUT "http://localhost:4200/api/v1/customer_board_comments/1" \
-H "Content-Type: application/json" \
-d '{
  "contents": "수정된 댓글 내용"
}'

##### Response

##### Header

| name | description | required |
|---|:---:|:---:|
| Content-Type | application/json | O |

##### Response Body

| name | type | description | required |
|---|:---:|:---:|:---:|
| id | Integer | 수정된 댓글 ID | O |

##### Example

json
HTTP/1.1 200 OK
Content-Type: application/json
{
  "id": 1
}

### - 댓글 삭제

설명
기존 댓글을 삭제합니다. 작성자만 삭제 가능합니다.

#### method: DELETE
#### URL: /api/v1/customer_board_comments/{id}

##### Request

##### Path Variables

| name | type | description | required |
|---|:---:|:---:|:---:|
| id | Integer | 댓글 ID | O |

##### Example

```bash
curl -X DELETE "http://localhost:4200/api/v1/customer_board_comments/1"

##### Response

##### Header

| name | description | required |
|---|:---:|:---:|

##### Example

HTTP/1.1 204 No Content

<h2 style='background-color: rgba(55, 55, 55, 0.2); text-align: center'>신고 모듈</h2>
댓글 신고와 관련된 REST API 모듈입니다.

url: /api/v1/customer_board_comment_reports


### - 댓글 신고

설명
댓글을 신고합니다. 로그인한 사용자만 신고 가능합니다.

#### method: POST
#### URL: /api/v1/customer_board_comment_reports

##### Request

##### Header

| name | description | required |
|---|:---:|:---:|
| Content-Type | application/json | O |

##### Request Body

| name | type | description | required |
|---|:---:|:---:|:---:|
| commentId | Integer | 신고할 댓글 ID | O |
| reason | String | 신고 사유 | O |

##### Example

```bash
curl -X POST "http://localhost:4200/api/v1/customer_board_comment_reports" \
-H "Content-Type: application/json" \
-d '{
  "commentId": 1,
  "reason": "신고 사유"
}'

##### Response

##### Header

| name | description | required |
|---|:---:|:---:|
| Content-Type | application/json | O |

##### Response Body

| name | type | description | required |
|---|:---:|:---:|:---:|
| id | Integer | 생성된 신고 ID | O |

##### Example

```json
HTTP/1.1 201 Created
Content-Type: application/json
{
  "id": 1
}