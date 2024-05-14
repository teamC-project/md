#### - 채팅방 리스트 불러오기
  
##### 설명

클라이언트로부터 Request Header의 Authorization 필드로 Bearer 토큰을 포함하여 요청을 보내면 작성일 기준 내림차순으로 게시물 리스트를 반환합니다. 게시물이 비밀글인 경우에는 작성자, 디자이너, 관리자만 해당 게시물과 댓글을 조회할 수 있습니다. 만약 불러오기에 실패하면 실패처리를 합니다. 인가 실패, 데이터베이스 에러가 발생할 수 있습니다. 

- method : **GET**  
- URL : **/communication/one-on-one/list** 

##### Request

###### Header

| name | description | required |
|---|:---:|:---:|
| Authorization | 인증에 사용될 Bearer 토큰 | O |

###### Example

```bash
curl -v -X GET "http://localhost:4200/api/v1/communication/one-on-one/list" \
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
| communicationOList | communicationOListItem[] | 소통 플랫폼 1:1 게시물 리스트 | O |

**communicationListItem**
| name | type | description | required |
|---|:---:|:---:|:---:|
| communicationONumber | int | 접수 번호 | O |
| title | String | 제목 | O |
| writerId | String | 작성자 아이디</br>(첫글자를 제외한 나머지 문자는 *) | O |
| writeDatetime | String | 작성일</br>(yy.mm.dd 형태) | O |

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
      "communicationONumber" : 1,
      "title": "헤어 스타일 추천해주세요", 
      "writerId": "d***",
      "writeDatetime": "23.05.5",
      "viewCount": 15
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


#### - 채팅방 불러오기  
  
##### 설명

클라이언트로부터 Request Header의 Authorization 필드로 Bearer 토큰을 포함하여 접수번호를 입력받고 요청을 보내면 해당하는 디자이너 게시물 데이터를 반환합니다. 만약 불러오기에 실패하면 실패처리를 합니다. 인가 실패, 데이터베이스 에러가 발생할 수 있습니다.

- method : **GET**  
- URL : **/communication/one-on-one/{communicationNumber}**  

##### Request

###### Header

| name | description | required |
|---|:---:|:---:|
| Authorization | 인증에 사용될 Bearer 토큰 | O |

###### Path Variable

| name | type | description | required |
|---|:---:|:---:|:---:|
| communicationONumber | int | 채팅방 번호 | O |

###### Example

```bash
curl -v -X GET "http://localhost:4200/api/v1/communication/one-on-one/${communicationNumber}" \
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
| communicationONumber | int | 채팅방 번호 | O |
| writerId | String | 작성자 아이디 | O |
| writeDatetime | String | 작성일</br>(yyyy.mm.dd 형태) | O |
| contents | String | 내용 | O |

###### Example

**응답 성공**
```bash
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
{
  "code": "SU",
  "message": "Success.",
  "communicationONumber": ${communicationONumber},
  "writerId": "${writerId}",
  "writeDatetime": "${writeDatetime}",
  "contents": "${contents}",
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

**응답 : 실패 (존재하지 않는 채팅방)**
```bash
HTTP/1.1 400 Bad Request
Content-Type: application/json;charset=UTF-8
{
  "code": "NC",
  "message": "No Exist Communication."
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