
<h1 style='background-color: rgba(55, 55, 55, 0.4); text-align: center'>API 명세서 </h1>

해당 API 명세서는 '채팅방'의 REST API를 명세하고 있습니다.

- Domain : <http://localhost:4200>  

***
  
<h2 style='background-color: rgba(55, 55, 55, 0.2); text-align: center'>채팅방 모듈</h2>

인증 및 인가와 관련된 REST API 모듈  
채팅방 생성, 메세지 작성, 삭제 리스트 보기 등의 API가 포함되어 있습니다.  
  
- url : **/api/v1/chat**

#### - 채팅방 생성
  
##### 설명

클라이언트로부터 Request Header의 Authorization 필드로 Bearer 토큰을 포함하여 제목을 입력받고 생성에 성공하면 성공처리를 합니다. 만약 생성에 실패하면 실패처리 됩니다. 인가 실패, 데이터베이스 에러 실패가 발생할 수 있습니다.

- method : **POST**  
- URL : **/room**  

##### Request

###### Header

| name | description | required |
|---|:---:|:---:|
| Authorization | 인증에 사용될 Bearer 토큰 | O |

###### Request Body

| name | type | description | required |
|---|:---:|:---:|:---:|
| roomName | String | 채팅방 제목 | O |

###### Example

```bash
curl -v -X POST "http://localhost:4200/api/v1/chat/room" \
 -H "Authorization: Bearer {JWT}" \
 -d "roomName"=`${roomName}` \
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

#### - 채팅방 리스트 불러오기  
  
##### 설명

클라이언트로부터 Request Header의 Authorization 필드로 Bearer 토큰을 포함하여 요청을 보내면 작성일 기준 내림차순으로 채팅방 리스트를 반환합니다. 만약 불러오기에 실패하면 실패처리를 합니다. 인가 실패, 데이터베이스 에러가 발생할 수 있습니다.

- method : **GET**  
- URL : **/rooms** 

##### Request

###### Header

| name | description | required |
|---|:---:|:---:|
| Authorization | 인증에 사용될 Bearer 토큰 | O |

###### Example

```bash
curl -v -X GET "http://localhost:4200/api/v1/chat/rooms" \
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
| chatRoomList | chatRoomListItem[] | 채팅방 리스트 | O |

**chatRoomListItem**
| name | type | description | required |
|---|:---:|:---:|:---:|
| roomId | int | 채팅방 번호 | O |
| roomName | String | 제목 | O |

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
      "roomId" : 1,
      "roomName": "디자이너 A의 방"
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

**응답 : 실패 (존재하지 않는 채팅방)**
```bash
HTTP/1.1 400 Bad Request
Content-Type: application/json;charset=UTF-8
{
  "code": "NB",
  "message": "No Exist Board."
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

클라이언트로부터 Request Header의 Authorization 필드로 Bearer 토큰을 포함하여 채팅방 번호를 입력받고 요청을 보내면 해당하는 채팅방 데이터를 반환합니다. 만약 불러오기에 실패하면 실패처리를 합니다. 인가 실패, 데이터베이스 에러가 발생할 수 있습니다.

- method : **GET**  
- URL : **/`${roomId}`**

##### Request

###### Header

| name | description | required |
|---|:---:|:---:|
| Authorization | 인증에 사용될 Bearer 토큰 | O |

###### Path Variable

| name | type | description | required |
|---|:---:|:---:|:---:|
| roomId | int | 채팅방 번호 | O |

###### Example

```bash
curl -v -X GET "http://localhost:4200/api/v1/chat/`${roomId}`" \
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
| chatroomId | int | 채팅방 번호 | O |
| roomName | String | 제목 | O |
| customerId | String | 고객 아이디  | O |
| designerId | String | 디자이너아이디  | O |

###### Example

**응답 성공**
```bash
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
{
  "code": "SU",
  "message": "Success.",
  "chatroomId": `${roomId}`,
  "roomName": `${roomName}`,
  "customerId": `${customerId}`,
  "designerId": `${designerId}`
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



#### - 채팅방 삭제  
  
##### 설명

클라이언트로부터 Request Header의 Authorization 필드로 Bearer 토큰을 포함하여 게시물번호를 입력받고 요청을 보내면 해당하는 채팅방이 삭제됩니다. 만약 삭제에 실패하면 실패처리를 합니다. 인가 실패, 데이터베이스 에러가 발생할 수 있습니다.

- method : **DELETE**  
- URL : **/`${roomId}`**  

##### Request

###### Header

| name | description | required |
|---|:---:|:---:|
| Authorization | 인증에 사용될 Bearer 토큰 | O |

###### Path Variable

| name | type | description | required |
|---|:---:|:---:|:---:|
| roomId | int | 채팅방 번호 | O |

###### Example

```bash
curl -v -X DELETE "http://localhost:4000/api/v1/chat/`${roomId}`" \
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

#### - 채팅방 메세지 불러오기  
  
##### 설명

클라이언트로부터 Request Header의 Authorization 필드로 Bearer 토큰을 포함하여 채팅방 번호를 입력받고 요청을 보내면 해당하는 채팅방 메세지 데이터를 반환합니다. 만약 불러오기에 실패하면 실패처리를 합니다. 인가 실패, 데이터베이스 에러가 발생할 수 있습니다.

- method : **GET**  
- URL : **/`${roomId}`/messages**

##### Request

###### Header

| name | description | required |
|---|:---:|:---:|
| Authorization | 인증에 사용될 Bearer 토큰 | O |

###### Path Variable

| name | type | description | required |
|---|:---:|:---:|:---:|
| roomId | int | 채팅방 번호 | O |

###### Example

```bash
curl -v -X GET "http://localhost:4200/api/v1/chat/`${roomId}`/messages" \
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
| chatroomId | int | 채팅방 번호 | O |
| messageId | int | 메세지 번호 | O |
| roomName | String | 제목 | O |
| senderId | String | 메세지 보내는 아이디  | O |
| message | String | 메세지 내용 | O |

###### Example

**응답 성공**
```bash
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
{
  "code": "SU",
  "message": "Success.",
  "chatroomId": `${roomId}`,
  "messageId": `${messageId}`,
  "roomName": `${roomName}`,
  "senderId": `${senderId}`,
  "message": `${message}`
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
