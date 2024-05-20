
<h1 style='background-color: rgba(55, 55, 55, 0.4); text-align: center'> 관리자 API 명세서 </h1>

해당 API 명세서는 '관리자'의 REST API를 명세하고 있습니다.

- Domain : <http://localhost:4200>  

***
  
<h2 style='background-color: rgba(55, 55, 55, 0.2); text-align: center'>헤어 트렌드  모듈</h2>

인증 및 인가와 관련된 REST API 모듈  
게시물 작성, 수정, 삭제 리스트 보기 의 API가 포함되어 있습니다.  
  
- url : /api/v1/trend_board

#### -  헤어트렌드 게시판 게시물 작성  
  
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
| trendBoardTitle | String | 헤어 트렌드 게시물 제목 | O |
| trendBoardContents | String | 헤어트렌드 게시물 내용 | O |



###### Example

```bash
curl -v -X POST "http://localhost:4200/api/v1/trend_board/" \
 -H "Authorization: Bearer {JWT}" \
 -d "trendBoardTitle={trendBoardtitle}" \
 -d "trendBoardContents={trendBoardcontents}"
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

#### - 헤어트렌드 게시판  전체 게시물 리스트 불러오기  
  
##### 설명

클라이언트로부터 Request Header의 Authorization 필드로 Bearer 토큰을 포함하여 요청을 보내면 작성일 기준 내림차순으로 게시물 리스트를 반환합니다. 만약 불러오기에 실패하면 실패처리를 합니다. 인가 실패, 데이터베이스 에러가 발생할 수 있습니다.

- method : **GET**  
- URL : **/list** 

##### Request

###### Header

| name | description | required |
|---|:---:|:---:|
| Authorization | 인증에 사용될 Bearer 토큰 | O |

###### Example

```bash
curl -v -X GET "http://localhost:4200/api/v1/trend_board/list" \
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
| trendboardList | trendBoardListItem[] | 헤어 트렌드 게시물 리스트 | O |

**trendBoardListItem**
| name | type | description | required |
|---|:---:|:---:|:---:|
| trendBoardNumber | int | 헤어트렌드 게시판 게시글 번호 | O |
| trendBoardTitle | String | 제목 | O |
| trendBoardTitleImage | String | 게시글 썸네일 이미지 | O |
| trendBoardWriterId | String | 작성자 아이디</br>(첫글자를 제외한 나머지 문자는 *) | O |
| trendBoardWriteDatetime | String | 작성일</br>(yy.mm.dd 형태) | O |
| trendBoardLikeCount | int | 좋아요 | O |
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
      "trendBoardNumber " : 1,
      "trendBoardTitle": "요즘 뜨는 헤어 스타일은 이거 입니다.", 
      "trendBoardTitleImage" : "hairImage01.jpg",
      "trendBoardWriterId": "d***",
      "trendBoardWriteDatetime": "23.05.15",
      "trendBoardLikeCount": 15
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


#### - 헤어 트렌드 게시판 게시물 검색 리스트 불러오기  
  
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
curl -v -X GET "http://localhost:4200/api/v1/trend_board/list/search?word=${searchWord}" \
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
| trendBoardList | trendBoardListItem[] | 헤어트렌드 게시판 게시물 리스트 | O |

**trendBoardListItem**
| name | type | description | required |
|---|:---:|:---:|:---:|
| trendBoardNumber | int | 헤어트렌드 게시판 게시글 번호 | O |
| trendBoardTitle | String | 제목 | O |
| trendBoardTitleImage | String | 게시글 썸네일 이미지 | O |
| trendBoardWriterId | String | 작성자 아이디</br>(첫글자를 제외한 나머지 문자는 *) | O |
| trendBoardWriteDatetime | String | 작성일</br>(yy.mm.dd 형태) | O |
| trendBoardLikeCount | int | 좋아요 | O |

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
      "trendBoardNumber " : 1,
      "trendBoardTitle": "요즘 뜨는 헤어 스타일은 이거 입니다.", 
      "trendBoardTitleImage" : "hairImage01.jpg",
      "trendBoardTrendBoardWriterId": "d***",
      "trendBoardWriteDatetime": "23.05.15",
      "trendBoardLikeCount": 15
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


#### - 헤어 트렌드 게시판 게시물 불러오기  
  
##### 설명

클라이언트로부터 Request Header의 Authorization 필드로 Bearer 토큰을 포함하여 헤어 트렌드 게시물 번호를 입력받고 요청을 보내면 해당하는 헤어트렌드 게시물 데이터를 반환합니다. 만약 불러오기에 실패하면 실패처리를 합니다. 인가 실패, 데이터베이스 에러가 발생할 수 있습니다.

- method : **GET**  
- URL : **/{trend_boardNumber}**  

##### Request

###### Header

| name | description | required |
|---|:---:|:---:|
| Authorization | 인증에 사용될 Bearer 토큰 | O |

###### Path Variable

| name | type | description | required |
|---|:---:|:---:|:---:|
| trendBoardNumber | int | 헤어트렌드 게시물 번호 | O |

###### Example

```bash
curl -v -X GET "http://localhost:4200/api/v1/trend_board/${trend_boardNumber}" \
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
| trendBoardNumber | int | 헤어트렌드 게시판 게시글 번호 | O |
| trendBoardTitle | String | 제목 | O |
| trendBoardTitleImage | String | 게시글 썸네일 이미지 | O |
| trendBoardContents | String | 게시글 내용 | O |
| trendBoardWriterId | String | 작성자 아이디</br>(첫글자를 제외한 나머지 문자는 *) | O |
| trendBoardWriteDatetime | String | 작성일</br>(yy.mm.dd 형태) | O |
| trendBoardLikeCount | int | 좋아요 | O |

###### Example

**응답 성공**
```bash
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
{
  "code": "SU",
  "message": "Success.",
  "trendBoardNumber": ${trendBoardNumber},
  "trendBoardTitle": "${trendBoardTitle}",
  "trendBoardTitleImage": "${trendBoardTitleImage}",
	"trendBoardContents": "${trendBoardContents}",
  "trendBoardWriterId": "${trendBoardWriterId}",
  "trendBoardWriteDatetime": "${trendBoardWriteDatetime}",
  "trendBoardLikeCount": ${trendBoardLikeCount},
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


#### - 헤어트렌드 게시물 좋아요 증가  
  
##### 설명

클라이언트로부터 Request Header의 Authorization 필드로 Bearer 토큰을 포함하여 게시물 번호를 입력받고 좋아요 버튼을 눌러 요청을 보내면 해당하는 헤어트렌드 게시물의 좋아요 수를 증가합니다. 만약 증가에 실패하면 실패처리를 합니다. 인가 실패, 데이터베이스 에러가 발생할 수 있습니다.

- method : **PATCH**  
- URL : **/`${trend_boardNumber}`/increase_like_count**  

##### Request

###### Header

| name | description | required |
|---|:---:|:---:|
| Authorization | 인증에 사용될 Bearer 토큰 | O |

###### Path Variable

| name | type | description | required |
|---|:---:|:---:|:---:|
| trendBoardNumber | int | 헤어트렌드 게시물 번호 | O |

###### Example

```bash
curl -v -X PATCH "http://localhost:4200/api/v1/trend_board/{trend_boardNumber}/increase_like_count$" \
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


#### - 헤어 트렌드 게시물 답글 작성  
  
##### 설명

클라이언트로부터 Request Header의 Authorization 필드로 Bearer 토큰을 포함하여 게시물 번호와 답글 내용을 입력받고 요청을 보내면 해당하는 트렌드 게시물의 답글이 작성됩니다. 만약 증가에 실패하면 실패처리를 합니다. 인가 실패, 데이터베이스 에러가 발생할 수 있습니다.

- method : **POST**  
- URL : **/{trend_boardNumber}/comment**  

##### Request

###### Header

| name | description | required |
|---|:---:|:---:|
| Authorization | 인증에 사용될 Bearer 토큰 | O |

###### Path Variable

| name | type | description | required |
|---|:---:|:---:|:---:|
| trendBoardNumber | int | 트렌드 게시물 번호 | O |

###### Request Body

| name | type | description | required |
|---|:---:|:---:|:---:|
| trendBoardcomment | String | 답글 내용 | O |
| trendBoardcomment | String | 답글 내용 | O |
| trendBoardcomment | String | 답글 내용 | O |

###### Example

```bash
curl -v -X POST "http://localhost:4200/api/v1/trend_board/${trend_boardNumber}/comment" \
 -H "Authorization: Bearer {JWT}" \
 -d "trendBoardcomment=`${trendBoardcomment}`"
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

---

#### - 헤어트렌드 게시물 삭제

##### 설명

클라이언트로부터 Request Header의 Authorization 필드로 Bearer 토큰을 포함하여 트렌드 게시물 번호를 입력받고 요청을 보내면 해당하는 헤어트렌드  게시물이 삭제됩니다. 만약 삭제에 실패하면 실패처리를 합니다. 인가 실패, 데이터베이스 에러가 발생할 수 있습니다.

- method : **DELETE**
- URL : **/{trendBoardNumber}**

##### Request

###### Header

| name          |        description        | required |
| ------------- | :-----------------------: | :------: |
| Authorization | 인증에 사용될 Bearer 토큰 |    O     |

###### Path Variable

| name            | type | description | required |
| --------------- | :--: | :---------: | :------: |
| trendBoardNumber | int  | 헤어 트렌드 게시물 번호  |    O     |

###### Example

```bash
curl -v -X POST "http://localhost:4200/api/v1/trend_board/${trendBoardNumber}" \
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

#### - 헤어 트렌드 게시판  게시물  수정

##### 설명

클라이언트로부터 Request Header의 Authorization 필드로 Bearer 토큰을 포함하여 헤어 트렌드 게시판 게시물 번호, 제목, 내용을 입력받고 수정에 성공하면 성공처리를 합니다. 만약 수정에 실패하면 실패처리 됩니다. 인가 실패, 데이터베이스 에러, 데이터 유효성 검사 실패가 발생할 수 있습니다.

- method : **PUT**
- URL : **/{trendBoardNumber}**

##### Request

###### Header

| name          |        description        | required |
| ------------- | :-----------------------: | :------: |
| Authorization | 인증에 사용될 Bearer 토큰 |    O     |

###### Path Variable

| name            | type |   description    | required |
| --------------- | :--: | :--------------: | :------: |
| trendBoardNumber | int  | 헤어트렌드 게시물 번호 |    O     |

###### Request Body

| name     |  type  | description | required |
| -------- | :----: | :---------: | :------: |
| trendBoardTitle    | String |  Q&A 제목   |    O     |
| trendBoardContents | String |  Q&A 내용   |    O     |

###### Example

```bash
curl -v -X PUT "http://localhost:4200/api/v1/trend_board/{trendBoardNumber}" \
 -H "Authorization: Bearer {JWT}" \
 -d "trendBoardTitle"=`${trendBoardTitle}` \
 -d "contents"=`${trendBoardContents}`
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

**응답 : 실패 (답변 완료된 게시물)**

```bash
HTTP/1.1 400 Bad Request
Content-Type: application/json;charset=UTF-8
{
  "code": "WC",
  "message": "Written Comment."
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

<h2 style='background-color: rgba(55, 55, 55, 0.2); text-align: center'>Q&A 게시판  모듈</h2>

인증 및 인가와 관련된 REST API 모듈  
게시물 작성, 수정, 삭제 리스트 보기 의 API가 포함되어 있습니다.  
  
- url : /api/v1/qna_board

#### - Q&A 게시물 작성

##### 설명

클라이언트로부터 Request Header의 Authorization 필드로 Bearer 토큰을 포함하여 제목, 내용을 입력받고 작성에 성공하면 성공처리를 합니다. 만약 작성에 실패하면 실패처리 됩니다. 인가 실패, 데이터베이스 에러, 데이터 유효성 검사 실패가 발생할 수 있습니다.

- method : **POST**
- URL : **/**

##### Request

###### Header

| name          |        description        | required |
| ------------- | :-----------------------: | :------: |
| Authorization | 인증에 사용될 Bearer 토큰 |    O     |

###### Request Body

| name     |  type  | description | required |
| -------- | :----: | :---------: | :------: |
| qnaBoardTitle    | String |  Q&A 제목   |    O     |
| qnaBoardContents | String |  Q&A 내용   |    O     |

###### Example

```bash
curl -v -X POST "http://localhost:4200/api/v1/qna_board/" \
 -H "Authorization: Bearer {JWT}" \
 -d "qnaBoardTitle"=`${qnaBoardTitle}` \
 -d "qnaBoardContents"=`${qnaBoardContents}`
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


#### - Q&A 전체 게시물 리스트 불러오기

##### 설명

클라이언트로부터 Request Header의 Authorization 필드로 Bearer 토큰을 포함하여 요청을 보내면 작성일 기준 내림차순으로 게시물 리스트를 반환합니다. 만약 불러오기에 실패하면 실패처리를 합니다. 인가 실패, 데이터베이스 에러가 발생할 수 있습니다.

- method : **GET**
- URL : **/list**

##### Request

###### Header

| name          |        description        | required |
| ------------- | :-----------------------: | :------: |
| Authorization | 인증에 사용될 Bearer 토큰 |    O     |

###### Example

```bash
curl -v -X GET "http://localhost:4200/api/v1/qna_board/list" \
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
| qnaBoardList | qnaBoardListItem[] | Q&A 게시물 리스트 |    O     |

**qnaBoardListItem**
| name | type | description | required |
|---|:---:|:---:|:---:|
| qnaBoardNumber | int | 설문 게시판 게시물 번호 | O |
| qnaBoardStatus | boolean | 상태 | O |
| qnaBoardTitle | String | 제목 | O |
| qnaBoardWriterId | String | 작성자 아이디</br>(첫글자를 제외한 나머지 문자는 \*) | O |
| qnaBoardWriteDatetime | String | 작성일</br>(yy.mm.dd 형태) | O |
| qnaBoardViewCount | int | 조회수 | O |

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
      "qnaBoardNumber": 1,
      "qnaBoardStatus": false,
      "qnaBoardTitle": "테스트1",
      "qnaBoardWriterId": "s******",
      "qnaBoardWriteDatetime": "24.05.15",
      "qnaBoardViewCount": 13
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

#### - Q&A 검색 게시물 리스트 불러오기

##### 설명

클라이언트로부터 Request Header의 Authorization 필드로 Bearer 토큰을 포함하여 검색어를 입력받고 요청을 보내면 작성일 기준 내림차순으로 제목에 해당 검색어가 포함된 게시물 리스트를 반환합니다. 만약 불러오기에 실패하면 실패처리를 합니다. 인가 실패, 데이터베이스 에러가 발생할 수 있습니다.

- method : **GET**
- URL : **/list/{searchWord}**

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
curl -v -X GET "http://localhost:4200/api/v1/qna_board/list/search?=${searchWord}" \
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
| qnaboardList | qnaBoardListItem[] | Q&A 게시물 리스트 |    O     |

**qnaBoardListItem**
| name | type | description | required |
|---|:---:|:---:|:---:|
| qnaBoardNumber | int | Q&A 게시물 번호 | O |
| qnaBoardStatus | boolean | 상태 | O |
| qnaBoardTitle | String | 제목 | O |
| qnaBoardWriterId | String | 작성자 아이디</br>(첫글자를 제외한 나머지 문자는 \*) | O |
| qnaBoardWriteDatetime | String | 작성일</br>(yy.mm.dd 형태) | O |
| qnaBoardViewCount | int | 조회수 | O |

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
      "qnaBoardNumber": 1,
      "qnaBoardStatus": false,
      "qnaBoardTitle": "테스트1",
      "qnaBoardWriterId": "s******",
      "qnaBoardWriteDatetime": "24.05.02",
      "qnaBoardViewCount": 0
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

#### - Q&A 게시물 불러오기

##### 설명

클라이언트로부터 Request Header의 Authorization 필드로 Bearer 토큰을 포함하여 Q&A 게시물 번호를 입력받고 요청을 보내면 해당하는 Q&A 게시물 데이터를 반환합니다. 만약 불러오기에 실패하면 실패처리를 합니다. 인가 실패, 데이터베이스 에러가 발생할 수 있습니다.

- method : **GET**
- URL : **/{qnaBoardNumber}**

##### Request

###### Header

| name          |        description        | required |
| ------------- | :-----------------------: | :------: |
| Authorization | 인증에 사용될 Bearer 토큰 |    O     |

###### Path Variable

| name            | type | description | required |
| --------------- | :--: | :---------: | :------: |
| qnaBoardNumber | int  |  Q&A 게시물 번호  |    O     |

###### Example

```bash
curl -v -X GET "http://localhost:4200/api/v1/qna_board/${qnaBoardNumber}" \
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
| qnaBoardNumber |   int   |          Q&A 게시물 번호           |    O     |
| qnaBoardStatus          | boolean |             상태             |    O     |
| qnaBoardTitle           | String  |             제목             |    O     |
| qnaBoardWriterId        | String  |        작성자 아이디         |    O     |
| qnaBoardWriteDatetime   | String  | 작성일</br>(yyyy.mm.dd 형태) |    O     |
| qnaBoardViewCount       |   int   |            조회수            |    O     |
| qnaBoardContents        | String  |             내용             |    O     |
| qnaBoardComment         | String  |          답글 내용           |    X     |

###### Example

**응답 성공**

```bash
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
{
  "code": "SU",
  "message": "Success.",
  "qnaBoardNumber": ${qnaBoardNumber},
	"qnaBoardStatus": ${status},
  "qnaBoardTitle": "${title}",
  "qnaBoardWriterId": "${qnaBoardWriterId}",
  "qnaBoardWriteDatetime": "${writeDatetime}",
  "qnaBoardViewCount": ${viewCount},
  "qnaBoardContents": "${contents}",
  "qnaBoardComment": "${comment}"
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

#### - Q&A 게시물 조회수 증가

##### 설명

클라이언트로부터 Request Header의 Authorization 필드로 Bearer 토큰을 포함하여 Q&A 게시물 번호를 입력받고 요청을 보내면 해당하는 Q&A 게시물의 조회수를 증가합니다. 만약 증가에 실패하면 실패처리를 합니다. 인가 실패, 데이터베이스 에러가 발생할 수 있습니다.

- method : **PATCH**
- URL : **/{qnaBoardNumber}/increase_qna-view_count**

##### Request

###### Header

| name          |        description        | required |
| ------------- | :-----------------------: | :------: |
| Authorization | 인증에 사용될 Bearer 토큰 |    O     |

###### Path Variable

| name            | type | description | required |
| --------------- | :--: | :---------: | :------: |
| qnaBoardNumber | int  |  Q&A 게시글 번호  |    O     |

###### Example

```bash
curl -v -X PATCH "http://localhost:4200/api/v1/qna_board/{qnaBoardNumber}/increase_qnaview_count$" \
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

#### - Q&A 게시물 답글 작성

##### 설명

클라이언트로부터 Request Header의 Authorization 필드로 Bearer 토큰을 포함하여 Q&A 게시물 번호와 답글 내용을 입력받고 요청을 보내면 해당하는 Q&A 게시물의 답글이 작성됩니다. 만약 증가에 실패하면 실패처리를 합니다. 인가 실패, 데이터베이스 에러가 발생할 수 있습니다.

- method : **POST**
- URL : **/{qnaBoardNumber}/comment**

##### Request

###### Header

| name          |        description        | required |
| ------------- | :-----------------------: | :------: |
| Authorization | 인증에 사용될 Bearer 토큰 |    O     |

###### Path Variable

| name            | type | description | required |
| --------------- | :--: | :---------: | :------: |
| qnaBoardNumber | int  |  Q&A 게시글 번호  |    O     |

###### Request Body

| name    |  type  | description | required |
| ------- | :----: | :---------: | :------: |
| qnaBoardComment | String |  답글 내용  |    O     |

###### Example

```bash
curl -v -X POST "http://localhost:4200/api/v1/qna_board/${qnaBoardNumber}/qna_comment" \
 -H "Authorization: Bearer {JWT}" \
 -d "qnaBoardComment"=`${qnaBoardComment}`
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

**응답 : 실패 (이미 작성된 답글)**

```bash
HTTP/1.1 400 Bad Request
Content-Type: application/json;charset=UTF-8
{
  "code": "WC",
  "message": "Written Comment."
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

#### - Q&A 게시물 삭제

##### 설명

클라이언트로부터 Request Header의 Authorization 필드로 Bearer 토큰을 포함하여 Q&A 게시물 번호를 입력받고 요청을 보내면 해당하는 Q&A 게시물이 삭제됩니다. 만약 삭제에 실패하면 실패처리를 합니다. 인가 실패, 데이터베이스 에러가 발생할 수 있습니다.

- method : **DELETE**
- URL : **/{qnaBoardNumber}**

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
curl -v -X POST "http://localhost:4200/api/v1/qna_board/${qnaBoardNumber}" \
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

#### - Q&A 게시물 수정

##### 설명

클라이언트로부터 Request Header의 Authorization 필드로 Bearer 토큰을 포함하여 Q&A 게시물 번호, 제목, 내용을 입력받고 수정에 성공하면 성공처리를 합니다. 만약 수정에 실패하면 실패처리 됩니다. 인가 실패, 데이터베이스 에러, 데이터 유효성 검사 실패가 발생할 수 있습니다.

- method : **PUT**
- URL : **/{qnaBoardNumber}**

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
| qnaBoardTitle    | String |  Q&A 제목   |    O     |
| qnaBoardContents | String |  Q&A 내용   |    O     |

###### Example

```bash
curl -v -X PUT "http://localhost:4200/api/v1/qna_board/{qnaBoardNumber}" \
 -H "Authorization: Bearer {JWT}" \
 -d "qnaBoardTitle"=`${qnaBoardTitle}` \
 -d "qnaBoardContents=`${qnaBoardContents}`
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

**응답 : 실패 (답변 완료된 게시물)**

```bash
HTTP/1.1 400 Bad Request
Content-Type: application/json;charset=UTF-8
{
  "code": "WC",
  "message": "Written Comment."
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


<h2 style='background-color: rgba(55, 55, 55, 0.2); text-align: center'>공지사항 게시판  모듈</h2>

인증 및 인가와 관련된 REST API 모듈  
게시물 작성, 수정, 삭제 리스트 보기 의 API가 포함되어 있습니다.  
  
- url : /api/v1/announcement_board

#### - 공지사항 게시물 작성

##### 설명

클라이언트로부터 Request Header의 Authorization 필드로 Bearer 토큰을 포함하여 제목, 내용을 입력받고 작성에 성공하면 성공처리를 합니다. 만약 작성에 실패하면 실패처리 됩니다. 인가 실패, 데이터베이스 에러, 데이터 유효성 검사 실패가 발생할 수 있습니다.

- method : **POST**
- URL : **/**

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
curl -v -X POST "http://localhost:4200/api/v1/announcement_board/" \
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


#### - 공지사항 전체 게시물 리스트 불러오기

##### 설명

클라이언트로부터 Request Header의 Authorization 필드로 Bearer 토큰을 포함하여 요청을 보내면 작성일 기준 내림차순으로 게시물 리스트를 반환합니다. 만약 불러오기에 실패하면 실패처리를 합니다. 인가 실패, 데이터베이스 에러가 발생할 수 있습니다.

- method : **GET**
- URL : **/list**

##### Request

###### Header

| name          |        description        | required |
| ------------- | :-----------------------: | :------: |
| Authorization | 인증에 사용될 Bearer 토큰 |    O     |

###### Example

```bash
curl -v -X GET "http://localhost:4200/api/v1/announcement_board/list" \
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

#### - 공지사항 검색 게시물 리스트 불러오기

##### 설명

클라이언트로부터 Request Header의 Authorization 필드로 Bearer 토큰을 포함하여 검색어를 입력받고 요청을 보내면 작성일 기준 내림차순으로 제목에 해당 검색어가 포함된 게시물 리스트를 반환합니다. 만약 불러오기에 실패하면 실패처리를 합니다. 인가 실패, 데이터베이스 에러가 발생할 수 있습니다.

- method : **GET**
- URL : **/list/search**

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
curl -v -X GET "http://localhost:4200/api/v1/announcement_board/list/search?word=${searchWord}" \
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
- URL : **/{announcementBoardNumber}/increase_announcement_view_count**

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
curl -v -X PATCH "http://localhost:4200/api/v1/announcement_board/${announcementBoardNumber}/increase_announcement_view_count$" \
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
curl -v -X POST "http://localhost:4200/api/v1/announcement_board/${announcementBoardNumber}" \
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


