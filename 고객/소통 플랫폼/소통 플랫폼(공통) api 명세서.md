소통 플랫폼(공통)api 명세서 0.3v
<h1 style='background-color: rgba(55, 55, 55, 0.4); text-align: center'>API 명세서</h1>

해당 API 명세서는 '소통플랫폼(공통)'의 REST API를 명세하고 있습니다.

-Domain: http://localhost:4200

***

<h2 style='background-color: rgba(55, 55, 55, 0.2); text-align: center'>소통 플랫폼 모듈</h2>

인증 및 인가와 관련된 REST API 모듈
게시물 작성, 수정, 삭제, 리스트 보기 등의 REST API가 포함되어 있습니다.

- url: /api/v1/customer_board



#### - 소통 플랫폼 게시판 전체 게시물 리스트 불러오기

##### 설명

클라이언트로부터 Request Header의 Authorization 필드로 Bearer 토큰을 포함하여 요청을 보내면 작성일 기준 내림차순으로 게시물 리스트를 반환합니다. 만약 불러오기에 실패하면 실패처리를 합니다. 인가 실패, 데이터베이스 에러가 발생할 수 있습니다.

- method : **GET**
- URL : **/list**

##### Request

###### Header

| name | description | required |
|---|:---:|:---:|
| Authorization | 인증에 사용될 Bearer 토큰 | O |

##### Example

```bash
curl -v -X GET "http://localhost:4200/api/v1/customer_board/list" \
 -H "Authorization: Bearer {JWT}"
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
| customerBoardList | customerBoardListItem[] | 소통 플랫폼 게시물 리스트 | O |

**customerBoardListItem**
| name | type | description | required |
|---|:---:|:---:|:---:|
| customerBoardNumber | int | 소통 플랫폼 게시판 게시글 번호 | O |
| customerBoardtitle | String | 제목 | O |
| customerBoardWriterId | String | 작성자 아이디</br>(첫글자를 제외한 나머지 문자는 *) | O |
| customerBoardWriteDatetime | String | 작성일</br>(yy.mm.dd 형태) | O |
| customerBoardViewCount | int | 조회수 | O |

##### Example

**응답 성공**
```bash
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
{
  "code": "SU",
  "message": "Success.",
  "boardList": [
    {
      "customerBoardNumber " : 1,
      "customerBoardtitle": "헤어 스타일 추천해주세요.", 
      "customerBoardWriterId": "d***",
      "customerBoardwriteDatetime": "23.05.15",
      "customerBoardViewCount": 15
    }, ...
  ]
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

```bash
HTTP/1.1 404 Not Found
Content-Type: application/json; charset=UTF-8
{
  "code": "PNF",
  "message": "Page Not Found."
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

#### - 소통 플랫폼 게시판 게시물 검색 리스트 불러오기  
  
##### 설명

클라이언트로부터 Request Header의 Authorization 필드로 Bearer 토큰을 포함하여 검색어를 입력받고 요청을 보내면 작성일 기준 내림차순으로 제목에 해당 검색어가 포함된 게시물 리스트를 반환합니다. 만약 불러오기에 실패하면 실패처리를 합니다. 인가 실패, 데이터베이스 에러가 발생할 수 있습니다.

- method : **GET**  
- URL : **/list/search**  

##### Request

###### Header

| name | description | required |
|---|:---:|:---:|
| Authorization | 인증에 사용될 Bearer 토큰 | O |

###### Request Param

| name | type | description | required |
|---|:---:|:---:|:---:|
| searchWord | String | 검색어 | O |

###### Example

```bash
curl -v -X GET "http://localhost:4200/api/v1/customer_board/list/search?word=${searchWord}" \
 -H "Authorization: Bearer {JWT}"
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
| customerBoardList | customerBoardListItem[] | 소통 플랫폼 게시물 리스트 | O |

**customerBoardListItem**
| name | type | description | required |
|---|:---:|:---:|:---:|
| customerBoardNumber | int | 게시물 번호 | O |
| customerBoardTitle | String | 제목 | O |
| customerBoardWriterId | String | 작성자 아이디</br>(첫글자를 제외한 나머지 문자는 *) | O |
| customerBoardWriteDatetime | String | 작성일</br>(yy.mm.dd 형태) | O |
| customerBoardViewCount | int | 조회수 | O |

###### Example

**응답 성공**
```bash
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
{
  "code": "SU",
  "message": "Success.",
  "boardList": [
    {
      "customerBoardNumber" : 1,
      "customerBoardTitle": "헤어 스타일 추천해주세요", 
      "customerBoardWriterId": "d***",
      "customerBoardWriteDatetime": "23.05.5",
      "customerBoardViewCount": 15
    }, ...
  ]
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

#### - 소통 플랫폼 게시물 불러오기  
  
##### 설명

클라이언트로부터 Request Header의 Authorization 필드로 Bearer 토큰을 포함하여 게시물번호를 입력받고 요청을 보내면 해당하는 소통 플랫폼 게시물 데이터를 반환합니다. 만약 불러오기에 실패하면 실패처리를 합니다. 인가 실패, 데이터베이스 에러가 발생할 수 있습니다.

- method : **GET**  
- URL : **/`${customerBoardNumber}`**

##### Request

###### Header

| name | description | required |
|---|:---:|:---:|
| Authorization | 인증에 사용될 Bearer 토큰 | O |

###### Path Variable

| name | type | description | required |
|---|:---:|:---:|:---:|
| customerBoardNumber | int | 게시물 번호 | O |

###### Example

```bash
curl -v -X GET "http://localhost:4200/api/v1/customer_board/`${customerBoardNumber}`" \
 -H "Authorization: Bearer {JWT}"
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
| customerBoardNumber | int | 게시물 번호 | O |
| customerBoardTitle | String | 제목 | O |
| customerBoardWriterId | String | 작성자 아이디 | O |
| customerBoardWriteDatetime | String | 작성일</br>(yyyy.mm.dd 형태) | O |
| customerBoardViewCount | int | 조회수 | O |
| customerBoardContents | String | 내용 | O |
| customerBoardComment | String | 답글 내용 | X |

###### Example

**응답 성공**
```bash
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
{
  "code": "SU",
  "message": "Success.",
  "customerBoardNumber": `${customerBoardNumber}`,
  "customerBoardTitle": `${customerBoardTitle}`,
  "customerBoardWriterId": `${customerBoardWriterId}`,
  "customerBoardWriteDatetime": `${customerBoardWriteDatetime}`,
  "customerBoardViewCount": `${customerBoardViewCount}`,
  "customerBoardContents": `${customerBoardContents}`,
  "customerBoardComment": `${customerBoardComment}`
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

**응답 : 실패 (존재하지 않는 게시물)**
```bash
HTTP/1.1 400 Bad Request
Content-Type: application/json;charset=UTF-8
{
  "code": "NB",
  "message": "No Exist Board."
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


#### - 소통 플랫폼 게시물 조회수 증가  
  
##### 설명

클라이언트로부터 Request Header의 Authorization 필드로 Bearer 토큰을 포함하여 게시물번호를 입력받고 요청을 보내면 해당하는 소통 플랫폼 게시물의 조회수를 증가합니다. 만약 증가에 실패하면 실패처리를 합니다. 인가 실패, 데이터베이스 에러가 발생할 수 있습니다.

- method : **PATCH**  
- URL : **/`${customerBoardNumber}`/increase-view-count**  

##### Request

###### Header

| name | description | required |
|---|:---:|:---:|
| Authorization | 인증에 사용될 Bearer 토큰 | O |

###### Path Variable

| name | type | description | required |
|---|:---:|:---:|:---:|
| customerBoardNumber | int | 게시물 번호 | O |

###### Example

```bash
curl -v -X PATCH "http://localhost:4200/api/v1/customer_board/`${customerBoardNumber}`/increase-view-count$" \
 -H "Authorization: Bearer {JWT}"
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
  "message": "Success."
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

**응답 : 실패 (존재하지 않는 게시물)**
```bash
HTTP/1.1 400 Bad Request
Content-Type: application/json;charset=UTF-8
{
  "code": "NB",
  "message": "No Exist Board."
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

#### - 소통 플랫폼 게시물 답글 작성  
  
##### 설명

클라이언트로부터 Request Header의 Authorization 필드로 Bearer 토큰을 포함하여 게시물번호와 답글 내용을 입력받고 요청을 보내면 해당하는 소통 플랫폼 게시물의 답글이 작성됩니다. 만약 증가에 실패하면 실패처리를 합니다. 인가 실패, 데이터베이스 에러가 발생할 수 있습니다.

- method : **POST**  
- URL : **/{customerBoardNumber}/comment**  

##### Request

###### Header

| name | description | required |
|---|:---:|:---:|
| Authorization | 인증에 사용될 Bearer 토큰 | O |

###### Path Variable

| name | type | description | required |
|---|:---:|:---:|:---:|
| customerBoardNumber | int | 게시물 번호 | O |

###### Request Body

| name | type | description | required |
|---|:---:|:---:|:---:|
| customerBoardComment | String | 답글 내용 | O |

###### Example

```bash
curl -v -X POST "http://localhost:4200/api/v1/customer_board/${customerBoardNumber}/comment" \
 -H "Authorization: Bearer {JWT}" \
 -d "customerBoardComment={customerBoardComment}"
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
  "message": "Success."
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

**응답 : 실패 (존재하지 않는 게시물)**
```bash
HTTP/1.1 400 Bad Request
Content-Type: application/json;charset=UTF-8
{
  "code": "NB",
  "message": "No Exist Board."
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