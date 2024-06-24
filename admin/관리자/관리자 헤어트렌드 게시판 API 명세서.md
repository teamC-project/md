<h1 style='background-color: rgba(55, 55, 55, 0.4); text-align: center'> 관리자 API 명세서 </h1>

해당 API 명세서는 '관리자'의 REST API를 명세하고 있습니다.

- Domain : <http://localhost:4200>  

***
  
<h2 style='background-color: rgba(55, 55, 55, 0.2); text-align: center'>헤어 트렌드  모듈</h2>

인증 및 인가와 관련된 REST API 모듈  
게시물 작성, 수정, 삭제 리스트 보기 의 API가 포함되어 있습니다.  
  
- url : /api/v1/service/trend_board

#### - 헤어트렌드 게시판  전체 게시물 리스트 불러오기  
  
##### 설명

클라이언트로부터 Request Header의 Authorization 필드로 Bearer 토큰을 포함하여 요청을 보내면 작성일 기준 내림차순으로 게시물 리스트를 반환합니다. 만약 불러오기에 실패하면 실패처리를 합니다. 인가 실패, 데이터베이스 에러가 발생할 수 있습니다.

- method : **GET**  
- URL : **/** 

##### Request

###### Header

| name | description | required |
|---|:---:|:---:|
| Authorization | 인증에 사용될 Bearer 토큰 | O |

###### Example

```bash
curl -v -X GET "http://localhost:4200/api/v1/trend_board/" \
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
| trendBoardThumbnailImage | String | 게시글 썸네일 이미지 | O |
| trendBoardWriterId | String | 작성자 아이디</br>(첫글자를 제외한 나머지 문자는 *) | O |
| trendBoardWriteDatetime | String | 작성일</br>(yy.mm.dd 형태) | O |
| trendBoardLikeCount | int | 좋아요 | O |
| trendBoardViewCount | int | 조회수 | O |
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
      "trendBoardThumbnailImage" : "hairImage01.jpg",
      "trendBoardWriterId": "d***",
      "trendBoardWriteDatetime": "23.05.15",
      "trendBoardLikeCount": 15
			"trendBoardViewCount" : 0
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

#### -  헤어트렌드 게시판 게시물 작성  
  
##### 설명

클라이언트로부터 Request Header의 Authorization 필드로 Bearer 토큰을 포함하여 제목, 내용을 입력받고 작성에 성공하면 성공처리를 합니다. 만약 작성에 실패하면 실패처리 됩니다. 인가 실패, 데이터베이스 에러, 데이터 유효성 검사 실패가 발생할 수 있습니다.

- method : **POST**  
- URL : **/write**  

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
| trendBoardThumbnailImage | Image | 헤어트렌드 게시물 썸네일 이미지 | O |



###### Example

```bash
curl -v -X POST "http://localhost:4200/api/v1/trend_board/write" \
 -H "Authorization: Bearer {JWT}" \
 -d "trendBoardTitle={trendBoardtitle}" \
 -d "trendBoardContents={trendBoardcontents}"\
 -d "trendBoardThumbnailImage = {trendBoardThumbnailImage}"\
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

#### - 헤어 트렌드 게시판 게시물 검색 리스트 불러오기  
  
##### 설명

클라이언트로부터 Request Header의 Authorization 필드로 Bearer 토큰을 포함하여 검색어를 입력받고 요청을 보내면 작성일 기준 내림차순으로 제목에 해당 검색어가 포함된 게시물 리스트를 반환합니다. 만약 불러오기에 실패하면 실패처리를 합니다. 인가 실패, 데이터베이스 에러가 발생할 수 있습니다.

- method : **GET**  
- URL : **/search**  

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
| trendBoardViewCount | int | 조회수 | O |

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
      "trendBoardThumbnailImage" : "hairImage01.jpg",
      "trendBoardTrendBoardWriterId": "d***",
      "trendBoardWriteDatetime": "23.05.15",
      "trendBoardLikeCount": 15
			"trendBoardViewCount": : 0
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
- URL : **/`${trendBoardNumber}`**  

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
curl -v -X GET "http://localhost:4200/api/v1/trend_board/${trendBoardNumber}" \
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
| trendBoardContents | String | 게시글 내용 | O |
| trendBoardWriterId | String | 작성자 아이디</br>(첫글자를 제외한 나머지 문자는 *) | O |
| trendBoardWriteDatetime | String | 작성일</br>(yy.mm.dd 형태) | O |
| trendBoardLikeCount | int | 좋아요 | O |
| trendBoardViewCount | int | 조회수 | O |

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
   "trendBoardContents": "${trendBoardContents}",
  "trendBoardWriterId": "${trendBoardWriterId}",
  "trendBoardWriteDatetime": "${trendBoardWriteDatetime}",
  "trendBoardLikeCount": ${trendBoardLikeCount},
	"trendBoardViewCount" : ${trendBoardViewCount}
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


#### - 헤어트렌드 게시물 좋아요
  
##### 설명

클라이언트로부터 Request Header의 Authorization 필드로 Bearer 토큰을 포함하여 게시물 번호를 입력받고 좋아요 버튼을 눌러 요청을 보내면 해당하는 헤어트렌드 게시물의 좋아요 수를 증가합니다. 만약 증가에 실패하면 실패처리를 합니다. 인가 실패, 데이터베이스 에러가 발생할 수 있습니다.

- method : **PUT**  
- URL : **/`${trendBoardNumber}`/like**  

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
curl -v -X PUT "http://localhost:4200/api/v1/trend_board/`${trendBoardNumber}`/like" \
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


#### - 헤어트렌드 게시물 답글 작성  
  
##### 설명

클라이언트로부터 Request Header의 Authorization 필드로 Bearer 토큰을 포함하여 게시물번호와 작성자, 답글 내용, 작성일을 입력받고 요청을 보내면 해당하는 헤어트렌드 게시물의 답글이 작성됩니다. 만약 증가에 실패하면 실패처리를 합니다. 인가 실패, 데이터베이스 에러가 발생할 수 있습니다.

- method : **POST**  
- URL : **/`${trendBoardNumber}`/comment**  

##### Request

###### Header

| name | description | required |
|---|:---:|:---:|
| Authorization | 인증에 사용될 Bearer 토큰 | O |

###### Path Variable

| name | type | description | required |
|---|:---:|:---:|:---:|
| trendBoardNumber | int | 헤어 트렌드 게시물 번호 | O |

###### Request Body

| name | type | description | required |
|---|:---:|:---:|:---:|
| trendBoardCommentWriterId | String | 답글 작성자 | O |
| trendBoardCommentContents | String | 답글 내용 | O |
| trendBoardCommentWriteDatetime | String | 답글 작성일</br>(yy.mm.dd 형태) | O |

###### Example

```bash
curl -v -X POST "http://localhost:4200/api/v1/trend_board/`${trendBoardNumber}`/comment" \
 -H "Authorization: Bearer {JWT}" \
 -d "trendBoardCommentWriterId"=`${trendBoardCommnetWriterId}`
 -d "trendBoardCommentContents"=`${trendBoardCommnetContents}`
 -d "trendBoardCommentWriteDatetime"=`${trendBoardCommnetWriteDatetime}`
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

#### - 헤어트렌드  게시물 답글  수정

##### 설명

클라이언트로부터 Request Header의 Authorization 필드로 Bearer 토큰을 포함하여 헤어 트렌드 게시판 게시물 번호,  작성자, 답글 내용, 작성일을 입력받고 수정에 성공하면 성공처리를 합니다. 만약 수정에 실패하면 실패처리 됩니다. 인가 실패, 데이터베이스 에러, 데이터 유효성 검사 실패가 발생할 수 있습니다.

- method : **PUT**
- URL : **/`${trendBoardNumber}`/comment**

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
| trendBoardCommentWriterId | String | 답글 작성자 | O |
| trendBoardCommentContents | String | 답글 내용 | O |
| trendBoardCommentWriteDatetime | String | 답글 작성일</br>(yy.mm.dd 형태) | O |

###### Example

```bash
curl -v -X PUT "http://localhost:4200/api/v1/trend_board/`${trendBoardNumber}`/comment" \
 -H "Authorization: Bearer {JWT}" \
 -d "trendBoardCommentWriterId=`${trendBoardCommnetWriterId}`
 -d "trendBoardCommentContents"=`${trendBoardCommnetContents}`
 -d "trendBoardCommentWriteDatetime"=`${trendBoardCommnetWriteDatetime}`
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


***


#### - 헤어 트렌드 게시물 답글 삭제  
  
##### 설명

클라이언트로부터 Request Header의 Authorization 필드로 Bearer 토큰을 포함하여 게시물번호를 입력받고 요청을 보내면 해당하는 헤어 트렌드 게시물의 답글이 삭제됩니다. 만약 삭제에 실패하면 실패처리를 합니다. 인가 실패, 데이터베이스 에러가 발생할 수 있습니다.

- method : **DELETE**  
- URL : **/`${trendBoardNumber}`/comment**  

##### Request

###### Header

| name | description | required |
|---|:---:|:---:|
| Authorization | 인증에 사용될 Bearer 토큰 | O |

###### Path Variable

| name | type | description | required |
|---|:---:|:---:|:---:|
| trendBoardNumber | int | 헤어 트렌드 게시물 번호 | O |

###### Example

```bash
curl -v -X DELETE "http://localhost:4000/api/v1/trend_board/`${trendBoardNumber}`/comment" \
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
curl -v -X DELETE "http://localhost:4200/api/v1/trend_board/${trendBoardNumber}" \
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

#### - 헤어 트렌드 좋아요 누른 유저 리스트 조회하기
  
##### 설명

클라이언트로부터 Request Header의 Authorization 필드로 Bearer 토큰을 포함하여 헤어 트렌드 게시물 번호를 입력받고 요청을 보내면 해당하는 헤어트렌드 게시물 좋아요 데이터를 반환합니다. 만약 불러오기에 실패하면 실패처리를 합니다. 인가 실패, 데이터베이스 에러가 발생할 수 있습니다.

- method : **GET**  
- URL : **/`${trendBoardNumber}/like_list`**  

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
curl -v -X GET "http://localhost:4200/api/v1/trend_board/${trendBoardNumber}/like_list" \
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
| likeList | String | 트렌드 게시물 좋아요 누른 유저 리스트 | O | 

###### Example

**응답 성공**
```bash
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
{
"code" : "SU",
"message" : "Success.",
"likeList" : ["admin"] , 
0
"admin"
}


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
#### - 헤어트렌드 게시물 삭제

##### 설명

클라이언트로부터 Request Header의 Authorization 필드로 Bearer 토큰을 포함하여 트렌드 게시물 번호를 입력받고 요청을 보내면 해당하는 헤어트렌드  게시물 좋아요 리스트가 삭제됩니다. 만약 삭제에 실패하면 실패처리를 합니다. 인가 실패, 데이터베이스 에러가 발생할 수 있습니다.

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
curl -v -X DELETE "http://localhost:4200/api/v1/trend_board/${trendBoardNumber}/like_list" \
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