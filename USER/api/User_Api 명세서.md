User

<h1 style='background-color: rgba(55, 55, 55, 0.4); text-align: center'>USER API 명세서 </h1>

해당 API 명세서는 '헤어케어 디자이너 매칭 플랫폼 서비스'의 REST API를 명세하고 있습니다.

- Domain : <http://localhost:4200>  

***

<h2 style='background-color: rgba(55, 55, 55, 0.2); text-align: center'>User 모듈</h2>

인증 및 인가와 관련된 REST API 모듈  
고객_개인정보수정, 디자이너_개인정보수정, 비밀번호 변경, 회원탈퇴 의 API가 포함되어 있습니다.  
  
- url : /api/v1/auth

***

#### - 내 정보
  
##### 설명
클라이언트로부터 인증된 토큰을 보유한 고객만 내 정보 URL을 통해 개인정보 수정, 비밀번호 재설정, 회원탈퇴 URL로 이동할 수 있는 버튼 통해 해당 페이지로 이동할 수 있습니다. 이동에 성공하면 성공처리 하고 만약 토큰을 인증받지 못한면 실패처리가 됩니다. 데이터베이스 에러가 발생할 수 있다.

- method : **GET**  
- URL : **/my_page**  

##### Request

###### Header

| name | description | required |
|---|:---:|:---:|
| Authorization | 인증에 사용될 Bearer 토큰 | O |

###### Request Body

| name | type | description | required |
|---|:---:|:---:|:---:|

###### Example

```bash
curl -v -X GET "http://localhost:4200/api/v1/user/my_page" \
 -H "Authorization: Bearer {JWT}" \
```

##### Response

###### Header

| name | description | required |
|---|:---:|:---:|
| Content-Type | 반환하는 Response Body의 Content Type (application/json) | O |

###### Response Body

| name | type | description | required |
|---|:---:|:---:|:---:|
| code | String | 응답 코드 | O |
| message | String | 응답 메시지 | O |


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
#### - 사용자_개인정보수정
  
##### 설명
클라이언트로부터 Request Header의 Authorization 필드로 Bearer 토큰을 포함하여 클라이언트로부터 고객 | 디자이너는 회원가입 시에 입력했던  비밀번호를 제외한 정보를 띄우고 아이디는 수정을 하지 못하도록 합니다. 성별, 연령대 값을 입력받고 디자이너는 면허증 사진 파일, 업체명을 추가로 입력받습니다. 정상적으로 개인정보 수정이 완료되면 성공처리를 합니다.
만약 작성에 실패하면 실패처리 됩니다. 인가 실패, 데이터베이스 에러, 데이터 유효성 검사 실패가 발생할 수 있습니다.

- method : **PUT**  
- URL : **/info_customer** , **/info_designer**

##### Request

###### Header

| name | description | required |
|---|:---:|:---:|
| Authorization | 인증에 사용될 Bearer 토큰 | O |

###### Request Body

| name | type | description | required |
|---|:---:|:---:|:---:|
| userId | String | 사용자 아이디 | O |
| userEmail | String | 사용자 이메일 (이메일 형태의 데이터) | O |
| userGender | String | 수정할 성별 | O  |
| userAge | Int | 수정할 나이 | O |
| type | customer, desginer, admin |  

###### Example

```bash
curl -v -X PUT "http://localhost:4200/api/v1/user/update" \
 -H "Authorization: Bearer {JWT}" \
 -d "userId=service123" \
 -d "userPassword=Pa55w0rd" \
 -d "userEmail=email@email.com" \
 -d "authNumber=0123" \
 -d "userAge":"20" \
 -d "userGender":"male"
```

##### Response

###### Header

| name | description | required |
|---|:---:|:---:|
| Content-Type | 반환하는 Response Body의 Content Type (application/json) | O |

###### Response Body

| name | type | description | required |
|---|:---:|:---:|:---:|
| code | String | 응답 코드 | O |
| message | String | 응답 메시지 | O |


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
  "message": "Varidation Failed."
}
```

**응답 : 실패 (이메일 인증 실패)**
```bash
HTTP/1.1 401 Unauthorized
Content-Type: application/json;charset=UTF-8
{
  "code": "AF",
  "message": "Authentication Failed."
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

### - 비밀번호 재설정

##### 설명
클라이언트로부터 고겍 | 디자이너의 비밀번호 정보를 받아와 현재 비밀번호 인증을 받고, 새 비밀번호와 새 비밀번호 확인을 통해 비밀번호 재설정을 한다. 현재 비밀번호 인증 실패, 새 비밀번호와 새비밀번호 확인이 일치하지 않으면 재설정에 실패 합니다. 데이터베이스 오류가 발생할 수 있다.

- method: **PUT**
- URL: **/update-user-password**

##### Request

###### Header

| name | description | required |
|---|:---:|:---:|
| Authorization | 인증에 사용될 Bearer 토큰 | O |

###### Request Body

| name | type | description | required |
|---|:---:|:---:|:---:|
| userPassword | String | 사용자 비밀번호 (영문+숫자 8~13자) | O |

###### Example

```bash
curl -v -X PUT "http://localhost:4200/api/v1/user/change-user-password" \
 -d '{"userPassword":"qwe123"}' \
 ```
##### Response

###### Header

| name | description | required |
|---|:---:|:---:|
| Content-Type | 반환하는 Response Body의 Content Type (application/json) | O |

###### Response Body

| name | type | description | required |
|---|:---:|:---:|:---:|
| code | String | 응답 코드 | O |
| message | String | 응답 메시지 | O |


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

**응답 : 실패 (유효성 검사 실패)**

```bash
HTTP/1.1 400 bed request
Content-Type: application/json;charset=UTF-8
{
  "code": "VF",
  "message": "Velidation Failed."
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

#### - 회원탈퇴
  
##### 설명
클라이언트로부터 고객의 정보를 영구 삭제한다. 성공 시 고객의 정보는 영구삭제된다.
에러 시 에러 메시지를 반환 받는다. 데이터베이스 오류가 발생할 수 있다.

- method : **DELETE**  
- URL : **/user_delete**  

##### Request

###### Header

| name | description | required |
|---|:---:|:---:|
| Authorization | 인증에 사용될 Bearer 토큰 | O |

###### Request Body

| name | type | description | required |
|---|:---:|:---:|:---:|

###### Example

```bash
curl -v -X POST "http://localhost:4200/api/v1/user/user_delete" \
 -H "Authorization: Bearer {JWT}" \
```

##### Response

###### Header

| name | description | required |
|---|:---:|:---:|
| Content-Type | 반환하는 Response Body의 Content Type (application/json) | O |

###### Response Body

| name | type | description | required |
|---|:---:|:---:|:---:|
| code | String | 응답 코드 | O |
| message | String | 응답 메시지 | O |


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