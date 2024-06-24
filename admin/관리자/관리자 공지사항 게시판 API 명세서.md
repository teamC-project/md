
<h1 style='background-color: rgba(55, 55, 55, 0.4); text-align: center'> 관리자 API 명세서 </h1>

해당 API 명세서는 '관리자'의 REST API를 명세하고 있습니다.

- Domain : <http://localhost:4200>  

***
  

<h2 style='background-color: rgba(55, 55, 55, 0.2); text-align: center'>공지사항 게시판  모듈</h2>

인증 및 인가와 관련된 REST API 모듈  
게시물 작성, 수정, 삭제 리스트 보기 의 API가 포함되어 있습니다.  
  
- url : /api/v1/service/announcement_board

#### - 공지사항 전체 게시물 리스트 불러오기

##### 설명

클라이언트로부터 Request Header의 Authorization 필드로 Bearer 토큰을 포함하여 요청을 보내면 작성일 기준 내림차순으로 게시물 리스트를 반환합니다. 만약 불러오기에 실패하면 실패처리를 합니다. 인가 실패, 데이터베이스 에러가 발생할 수 있습니다.

- method : **GET**
- URL : **/**

##### Request

###### Header

| name          |        description        | required |
| ------------- | :-----------------------: | :------: |
| Authorization | 인증에 사용될 Bearer 토큰 |    O     |

###### Example

```bash
curl -v -X GET "http://localhost:4200/api/v1/announcement_board/" \
 -H "Authorization: Bearer {JWT}"
```

##### Response

###### Header

| name         |                       description                        | required |
| ------------ | :------------------------------------------------------: | :------: |
| Content-Type | 반환하는 Response Body의 Content Type (application/json) |    O     |

###### Response Body

| name      |      type       |    description    | required |
| --------- | :-------------: | :---------------: | :------: |
| code      |     String      |     결과 코드     |    O     |
| message   |     String      |    결과 메세지    |    O     |
| announceentBoardList | announcementBoardListItem[] | 공지사항 게시물 리스트 |    O     |

**announcementBoardListItem**
| name | type | description | required |
|---|:---:|:---:|:---:|
| announcementBoardNumber | int | 공지사항 게시물 번호 | O |
| announcementBoardTitle | String | 제목 | O |
| announcementBoardWriterId | String | 작성자 아이디</br>(첫글자를 제외한 나머지 문자는 \*) | O |
| announcementBoardWriteDatetime | String | 작성일</br>(yy.mm.dd 형태) | O |
| announcementBoardViewCount | int | 조회수 | O |

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
      "announcementBoardNumber": 1,
      "announcementBoardTitle": "테스트1",
      "announcementBoardWriterId": "s******",
      "announcementBoardWriteDatetime": "24.05.15",
      "announcementBoardViewCount": 13
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

**응답 : 실패 (데이터베이스 오류)**

```bash
HTTP/1.1 500 Internal Server Error
Content-Type: application/json;charset=UTF-8
{
  "code": "DBE",
  "message": "Database Error."
}
```

---

#### - 공지사항 게시물 작성

##### 설명

클라이언트로부터 Request Header의 Authorization 필드로 Bearer 토큰을 포함하여 제목, 내용을 입력받고 작성에 성공하면 성공처리를 합니다. 만약 작성에 실패하면 실패처리 됩니다. 인가 실패, 데이터베이스 에러, 데이터 유효성 검사 실패가 발생할 수 있습니다.

- method : **POST**
- URL : **/write**

##### Request

###### Header

| name          |        description        | required |
| ------------- | :-----------------------: | :------: |
| Authorization | 인증에 사용될 Bearer 토큰 |    O     |

###### Request Body

| name     |  type  | description | required |
| -------- | :----: | :---------: | :------: |
| announcement_board_title    | String | 공지사항 제목   |    O     |
| announcement_board_contents | String |  공지사항 내용   |    O     |

###### Example

```bash
curl -v -X POST "http://localhost:4200/api/v1/announcement_board/write" \
 -H "Authorization: Bearer {JWT}" \
 -d "announcementBoardTitle={announcementBoardTitle}" \
 -d "announcementBoardContents={announcementBoardContents}
```

##### Response

###### Header

| name         |                       description                        | required |
| ------------ | :------------------------------------------------------: | :------: |
| Content-Type | 반환하는 Response Body의 Content Type (application/json) |    O     |

###### Response Body

| name    |  type  | description | required |
| ------- | :----: | :---------: | :------: |
| code    | String |  결과 코드  |    O     |
| message | String | 결과 메세지 |    O     |

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

---


#### - 공지사항 검색 게시물 리스트 불러오기

##### 설명

클라이언트로부터 Request Header의 Authorization 필드로 Bearer 토큰을 포함하여 검색어를 입력받고 요청을 보내면 작성일 기준 내림차순으로 제목에 해당 검색어가 포함된 게시물 리스트를 반환합니다. 만약 불러오기에 실패하면 실패처리를 합니다. 인가 실패, 데이터베이스 에러가 발생할 수 있습니다.

- method : **GET**
- URL : **/search**

##### Request

###### Header

| name          |        description        | required |
| ------------- | :-----------------------: | :------: |
| Authorization | 인증에 사용될 Bearer 토큰 |    O     |

###### Request Param

| name       |  type  | description | required |
| ---------- | :----: | :---------: | :------: |
| searchWord | String |   검색어    |    O     |

###### Example

```bash
curl -v -X GET "http://localhost:4200/api/v1/announcement_board/search?word=${searchWord}" \
 -H "Authorization: Bearer {JWT}"
```

##### Response

###### Header

| name         |                       description                        | required |
| ------------ | :------------------------------------------------------: | :------: |
| Content-Type | 반환하는 Response Body의 Content Type (application/json) |    O     |

###### Response Body

| name      |      type       |    description    | required |
| --------- | :-------------: | :---------------: | :------: |
| code      |     String      |     결과 코드     |    O     |
| message   |     String      |    결과 메세지    |    O     |
| announcementBoardList | BoardListItem[] | 공지사항 게시물 리스트 |    O     |

**BoardListItem**
| name | type | description | required |
|---|:---:|:---:|:---:|
| announcementBoardNumber | int | 공지사항 게시물 번호 | O |
| announcementBoardTitle | String | 제목 | O |
| announcementBoardWriterId | String | 작성자 아이디</br>(첫글자를 제외한 나머지 문자는 \*) | O |
| announcementBoardWriteDatetime | String | 작성일</br>(yy.mm.dd 형태) | O |
| announcementBoardViewCount | int | 조회수 | O |

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
      "announcementBoardNumber": 1,
      "announcementBoardTitle": "테스트1",
      "announcementBoardWriterId": "s******",
      "announcementBoardWriteDatetime": "24.05.02",
      "announcementBoardViewCount": 0
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

---

#### - 공지사항 게시물 불러오기

##### 설명

클라이언트로부터 Request Header의 Authorization 필드로 Bearer 토큰을 포함하여 공지사항 게시물 번호를 입력받고 요청을 보내면 해당하는 Q&A 게시물 데이터를 반환합니다. 만약 불러오기에 실패하면 실패처리를 합니다. 인가 실패, 데이터베이스 에러가 발생할 수 있습니다.

- method : **GET**
- URL : **/{announcementBoardNumber}**

##### Request

###### Header

| name          |        description        | required |
| ------------- | :-----------------------: | :------: |
| Authorization | 인증에 사용될 Bearer 토큰 |    O     |

###### Path Variable

| name            | type | description | required |
| --------------- | :--: | :---------: | :------: |
| announcementBoardNumber | int  |  공지사항 게시물 번호  |    O     |

###### Example

```bash
curl -v -X GET "http://localhost:4200/api/v1/announcement_board/${announcementBoardNumber}" \
 -H "Authorization: Bearer {JWT}"
```

##### Response

###### Header

| name         |                       description                        | required |
| ------------ | :------------------------------------------------------: | :------: |
| Content-Type | 반환하는 Response Body의 Content Type (application/json) |    O     |

###### Response Body

| name            |  type   |         description          | required |
| --------------- | :-----: | :--------------------------: | :------: |
| code            | String  |          결과 코드           |    O     |
| message         | String  |         결과 메세지          |    O     |
| announcementBoardNumber |   int   |         공지사항 게시물 번호           |    O     |
| announcementBoardTitle           | String  |             제목             |    O     |
| announcementBoardWriterId        | String  |        작성자 아이디         |    O     |
| announcementBoardWriteDatetime   | String  | 작성일</br>(yyyy.mm.dd 형태) |    O     |
| announcementBoardViewCount       |   int   |            조회수            |    O     |
| announcementBoardContents        | String  |             내용             |    O     |

###### Example

**응답 성공**

```bash
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
{
  "code": "SU",
  "message": "Success.",
  "announcementBoardBoardNumber": ${announcementBoardBoardNumber},
  "announcementBoardTitle": "${announcementBoardTitle}",
  "announcementBoardQnaBoardWriterId": "${announcementBoardWriterId}",
  "announcementBoardWriteDateTime": "${announcementBoardWriteDatetime}",
  "announcementBoardViewCount": ${announcementBoardViewCount},
  "announcementBoardContents": "${announcementBoardContents}"
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

---

#### - 공지사항 게시물 조회수 증가

##### 설명

클라이언트로부터 Request Header의 Authorization 필드로 Bearer 토큰을 포함하여 공지사항 게시물 번호를 입력받고 요청을 보내면 해당하는 공지사항 게시물의 조회수를 증가합니다. 만약 증가에 실패하면 실패처리를 합니다. 인가 실패, 데이터베이스 에러가 발생할 수 있습니다.

- method : **PATCH**
- URL : **/{announcementBoardNumber}/increase_view_count**

##### Request

###### Header

| name          |        description        | required |
| ------------- | :-----------------------: | :------: |
| Authorization | 인증에 사용될 Bearer 토큰 |    O     |

###### Path Variable

| name            | type | description | required |
| --------------- | :--: | :---------: | :------: |
| announcementBoardNumber | int  |  Q&A 게시글 번호  |    O     |

###### Example

```bash
curl -v -X PATCH "http://localhost:4200/api/v1/announcement_board/${announcementBoardNumber}/increase_view_count$" \
 -H "Authorization: Bearer {JWT}"
```

##### Response

###### Header

| name         |                       description                        | required |
| ------------ | :------------------------------------------------------: | :------: |
| Content-Type | 반환하는 Response Body의 Content Type (application/json) |    O     |

###### Response Body

| name    |  type  | description | required |
| ------- | :----: | :---------: | :------: |
| code    | String |  결과 코드  |    O     |
| message | String | 결과 메세지 |    O     |

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

---

#### - 공지사항 게시물 삭제

##### 설명

클라이언트로부터 Request Header의 Authorization 필드로 Bearer 토큰을 포함하여 공지사항 게시물 번호를 입력받고 요청을 보내면 해당하는 Q&A 게시물이 삭제됩니다. 만약 삭제에 실패하면 실패처리를 합니다. 인가 실패, 데이터베이스 에러가 발생할 수 있습니다.

- method : **DELETE**
- URL : **/`${announcementBoardNumber}`**

##### Request

###### Header

| name          |        description        | required |
| ------------- | :-----------------------: | :------: |
| Authorization | 인증에 사용될 Bearer 토큰 |    O     |

###### Path Variable

| name            | type | description | required |
| --------------- | :--: | :---------: | :------: |
| qnaBoardNumber | int  | Q&A 게시글 번호  |    O     |

###### Example

```bash
curl -v -X DELETE "http://localhost:4200/api/v1/announcement_board/${announcementBoardNumber}" \
 -H "Authorization: Bearer {JWT}"
```

##### Response

###### Header

| name         |                       description                        | required |
| ------------ | :------------------------------------------------------: | :------: |
| Content-Type | 반환하는 Response Body의 Content Type (application/json) |    O     |

###### Response Body

| name    |  type  | description | required |
| ------- | :----: | :---------: | :------: |
| code    | String |  결과 코드  |    O     |
| message | String | 결과 메세지 |    O     |

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

---

#### - 공지사항 게시물 게시물 수정

##### 설명

클라이언트로부터 Request Header의 Authorization 필드로 Bearer 토큰을 포함하여 공지사항 게시물 번호, 제목, 내용을 입력받고 수정에 성공하면 성공처리를 합니다. 만약 수정에 실패하면 실패처리 됩니다. 인가 실패, 데이터베이스 에러, 데이터 유효성 검사 실패가 발생할 수 있습니다.

- method : **PUT**
- URL : **/`${announcementBoardNumber}`**
##### Request

###### Header

| name          |        description        | required |
| ------------- | :-----------------------: | :------: |
| Authorization | 인증에 사용될 Bearer 토큰 |    O     |

###### Path Variable

| name            | type |   description    | required |
| --------------- | :--: | :--------------: | :------: |
| qnaBoardNumber | int  | Q&A 게시물 번호 |    O     |

###### Request Body

| name     |  type  | description | required |
| -------- | :----: | :---------: | :------: |
| title    | String |  Q&A 제목   |    O     |
| contents | String |  Q&A 내용   |    O     |

###### Example

```bash
curl -v -X PUT "http://localhost:4200/api/v1/qna_board/{qnaBoardNumber}" \
 -H "Authorization: Bearer {JWT}" \
 -d "announceMentBoardTitle"=`${announceMentBoardTitle}` \
 -d "announceMentBoardContents"=`${announceMentBoardContents}`
```

##### Response

###### Header

| name         |                       description                        | required |
| ------------ | :------------------------------------------------------: | :------: |
| Content-Type | 반환하는 Response Body의 Content Type (application/json) |    O     |

###### Response Body

| name    |  type  | description | required |
| ------- | :----: | :---------: | :------: |
| code    | String |  결과 코드  |    O     |
| message | String | 결과 메세지 |    O     |

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

**응답 : 실패 (존재하지 않는 게시물)**

```bash
HTTP/1.1 400 Bad Request
Content-Type: application/json;charset=UTF-8
{
  "code": "NB",
  "message": "No Exist Board."
}
```

**응답 : 실패 (권한 없음)**

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

