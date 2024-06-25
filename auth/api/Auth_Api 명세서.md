Auth 

<h1 style='background-color: rgba(55, 55, 55, 0.4); text-align: center'>API 명세서 </h1>

해당 API 명세서는 '헤어케어 디자이너 매칭 플랫폼 서비스'의 REST API를 명세하고 있습니다.

- Domain : <http://localhost:4200>  

***

<h2 style='background-color: rgba(55, 55, 55, 0.2); text-align: center'>Auth 모듈</h2>

인증 및 인가와 관련된 REST API 모듈  
로그인, 회원가입, 소셜 로그인, 소셜 회원가입 등의 API가 포함되어 있습니다.  
  
- url : /api/v1/auth

***

#### - 회원가입 이메일 인증  
  
##### 설명

클라이언트로부터 이메일을 입력받아 해당하는 이메일이 이미 사용중인 이메일인지 확인하고 사용하고 있지 않은 이메일이라면 
4자리의 인증코드를 해당 이메일로 전송합니다. 이메일 전송이 성공적으로 종료되었으면 성공처리를 합니다. 
만약 중복된 이메일이거나 이메일 전송에 실패했으면 실패처리를 합니다. 데이터베이스 오류가 발생할 수 있습니다.

- method : **POST**  
- URL : **/email-auth**  

##### Request

###### Header

| name | description | required |
|---|:---:|:---:|

###### Request Body

| name | type | description | required |
|---|:---:|:---:|:---:|
| userEmail | String | 인증 번호를 전송할 사용자 이메일</br>(이메일 형태의 데이터) | O |
|  type | customer, desginer, admin |

###### Example

```bash
curl -v -X POST "http://localhost:4200/api/v1/auth/email-auth" \
 -d "userEmail=email@email.com"
```

##### Response

###### Header

| name | description | required |
|---|:---:|:---:|
| Content-Type | 반환하는 Response Body의 Content Type (application/json) | O |
| type | cutomer, desginer, admin |

###### Response Body

| name | type | description | required |
|---|:---:|:---:|:---:|
| code | String | 사용자의 아이디 | O |
| message | String | 사용자의 비밀번호 | O |
| type | cutomer, desginer, admin |

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

**응답 : 실패 (중복된 이메일)**
```bash
HTTP/1.1 400 Bad Request
Content-Type: application/json;charset=UTF-8
{
  "code": "DE",
  "message": "Duplicatied Email."
}
```

**응답 : 실패 (이메일 전송 실패)**
```bash
HTTP/1.1 500 Internal Server Error
Content-Type: application/json;charset=UTF-8
{
  "code": "MF",
  "message": "Mail send Failed."
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

#### - 아이디 비밀번호 찾기 이메일 인증  
  
##### 설명

클라이언트로부터 이메일을 입력받아 해당하는 이메일이 존재하는 이메일인지 확인하고 존재하는 이메일이라면 
4자리의 인증코드를 해당 이메일로 전송합니다. 이메일 전송이 성공적으로 종료되었으면 성공처리를 합니다. 
만약 중복된 이메일이거나 이메일 전송에 실패했으면 실패처리를 합니다. 데이터베이스 오류가 발생할 수 있습니다.

- method : **POST**  
- URL : **/found-email-auth**  

##### Request

###### Header

| name | description | required |
|---|:---:|:---:|

###### Request Body

| name | type | description | required |
|---|:---:|:---:|:---:|
| userEmail | String | 인증 번호를 전송할 사용자 이메일</br>(이메일 형태의 데이터) | O |
|  type | customer, desginer, admin |

###### Example

```bash
curl -v -X POST "http://localhost:4200/api/v1/auth/found-email-auth" \
 -d "userEmail=email@email.com"
```

##### Response

###### Header

| name | description | required |
|---|:---:|:---:|
| Content-Type | 반환하는 Response Body의 Content Type (application/json) | O |
| type | cutomer, desginer, admin |

###### Response Body

| name | type | description | required |
|---|:---:|:---:|:---:|
| code | String | 사용자의 아이디 | O |
| message | String | 사용자의 비밀번호 | O |
| type | cutomer, desginer, admin |

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

**응답 : 실패 (중복된 이메일)**
```bash
HTTP/1.1 400 Bad Request
Content-Type: application/json;charset=UTF-8
{
  "code": "DE",
  "message": "Duplicatied Email."
}
```

**응답 : 실패 (이메일 전송 실패)**
```bash
HTTP/1.1 500 Internal Server Error
Content-Type: application/json;charset=UTF-8
{
  "code": "MF",
  "message": "Mail send Failed."
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

#### - 이메일 인증 확인
  
##### 설명

클라이언트로부터 이메일과 인증 번호를 입력받아 해당하는 이메일에 전송한 인증번호와 일치하는지 확인합니다. 일치한다면 성공처리를 합니다. 만약 일치하지 않는다면 실패처리를 합니다. 데이터베이스 오류가 발생할 수 있습니다.

- method : **POST**  
- URL : **/email-auth-check**

##### Request

###### Header

| name | description | required |
|---|:---:|:---:|

###### Request Body

| name | type | description | required |
|---|:---:|:---:|:---:|
| userEmail | String | 인증 번호를 확인할 사용자 이메일 | O |
| authNumber | String | 인증 확인할 인증 번호 | O |
| type | cutomer, desginer, admin |

###### Example

```bash
curl -v -X POST "http://localhost:4200/api/v1/auth/email-auth-check" \
 -d "userEmail=email@email.com" \
 -d "authNumber=0123"
```

##### Response

###### Header

| name | description | required |
|---|:---:|:---:|
| Content-Type | 반환하는 Response Body의 Content Type (application/json) | O |
| type | customer, desginer, admin |

###### Response Body

| name | type | description | required |
|---|:---:|:---:|:---:|
| code | String | 사용자의 아이디 | O |
| message | String | 사용자의 비밀번호 | O |
| type | customer, desginer, admin |    

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

#### - 고객 회원가입
  
##### 설명

클라이언트로부터 공통 회원가입 URL에서 고객 회원가입 버튼을 클릭하여 고객 회원가입 URL로 이동합니다. 고객은 아이디, 비밀번호, 이메일, 인증번호, 연령대, 성별을 필수로 입력받아 회원가입 처리를 합니다. 정상적으로 회원가입으로 완료하면 회원가입 성공 처리를 합니다.
만약 중복된 아이디, 중복된 이메일, 인증번호 불일치가 발생하면 실패처리를 합니다. 데이터베이스 오류가 발생할 수 있습니다.

- method : **POST**  
- URL : **/sign-up/customer**  

##### Request

###### Header

| name | description | required |
|---|:---:|:---:|

###### Request Body

| name | type | description | required |
|---|:---:|:---:|:---:|
| userId | String | 사용자 아이디 | O |
| userPassword | String | 사용자 비밀번호 (영문+숫자 8~13자) | O |
| userEmail | String | 사용자 이메일 (이메일 형태의 데이터) | O |
| authNumber | Int | 인증 확인할 인증 번호 | O |
| userGender | String |  선택한 성별 ("male", "female") | O |
| userAge | Int | 선택할 나이 | O |
| type | customer, desginer, admin |  

###### Example

```bash
curl -v -X POST "http://localhost:4200/api/v1/auth/sign-up/customer" \
 -d "userId=service123" \
 -d "userPassword=Pa55w0rd" \
 -d "userEmail=email@email.com" \
 -d "authNumber=0123"  \
 -d "userAge":"20대" \
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

**응답 : 실패 (중복된 아이디)**
```bash
HTTP/1.1 400 Bad Request
Content-Type: application/json;charset=UTF-8
{
  "code": "DI",
  "message": "Duplicatied Id."
}
```

**응답 : 실패 (중복된 이메일)**
```bash
HTTP/1.1 400 Bad Request
Content-Type: application/json;charset=UTF-8
{
  "code": "DE",
  "message": "Duplicatied Email."
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

#### - 디자이너 회원가입
  
##### 설명

클라이언트로부터 공통 회원가입 URL에서 다자이너 회원가입 버튼을 클릭하여  아이디, 비밀번호, 이메일, 인증번호, 연령대, 성별, 면허증 사진 파일, 업체명을 필수로 입력받아 회원가입 처리를 합니다. 정상적으로 회원가입이 완료되면 성공처리를 합니다. 
만약 중복된 아이디, 중복된 이메일, 인증번호 불일치가 발생하면 실패처리를 합니다. 데이터베이스 오류가 발생할 수 있습니다.

- method : **POST**  
- URL : **/sign-up/designer**  

##### Request

###### Header

| name | description | required |
|---|:---:|:---:|

###### Request Body

| name | type | description | required |
|---|:---:|:---:|:---:|
| userId | String | 사용자 아이디 | O |
| userPassword | String | 사용자 비밀번호 (영문+숫자 8~13자) | O |
| userEmail | String | 사용자 이메일 (이메일 형태의 데이터) | O |
| authNumber | Int | 인증 확인할 인증 번호 | O |
| userGender | String |  선택한 성별 ("male", "female") | O |
| userAge | Int | 선택할 나이 | O |
| designerImage | String | 면허증 사진 | X |  
| designerCompanyName | String | 업체명  |  X |
| type | customer, desginer, admin |  

###### Example

```bash
curl -v -X POST "http://localhost:4200/api/v1/auth/sign-up/designer" \
 -d "userId=service123" \
 -d "userPassword=Pa55w0rd" \
 -d "userEmail=email@email.com" \
 -d "authNumber=0123"  \
 -d "userAge=20" \
 -d "userGender=male" \
 -d "userCompanyName=hair" \
 -d "userImage=123.jgpe"
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

**응답 : 실패 (중복된 아이디)**
```bash
HTTP/1.1 400 Bad Request
Content-Type: application/json;charset=UTF-8
{
  "code": "DI",
  "message": "Duplicatied Id."
}
```

**응답 : 실패 (중복된 이메일)**
```bash
HTTP/1.1 400 Bad Request
Content-Type: application/json;charset=UTF-8
{
  "code": "DE",
  "message": "Duplicatied Email."
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

#### - 로그인  
  
##### 설명

클라이언트로부터 사용자 아이디와 평문의 비밀번호를 입력받고 아이디와 비밀번호가 일치한다면 성공처리가되며 access_token과 해당 토큰의 만료 기간을 반환합니다. 만약 아이디 혹은 비밀번호가 하나라도 틀리다면 실패 처리됩니다. 서버 에러, 데이터베이스 에러, 토큰 생성 에러가 발생할 수 있습니다.

- method : **POST**  
- URL : **/sign-in**  

##### Request

###### Header

| name | description | required |
|---|:---:|:---:|

###### Request Body

| name | type | description | required |
|---|:---:|:---:|:---:|
| userId | String | 사용자의 아이디 | O |
| userPassword | String | 사용자의 비밀번호 | O |
| type | customer, designer, admin |
###### Example

```bash
curl -v -X POST "http://localhost:4200/api/v1/auth/sign-in" \
 -d "userId=service123" \
 -d "userPassword=P!ssw0rd"
```

##### Response

###### Header

| name | description | required |
|---|:---:|:---:|
| Content-Type | 반환하는 Response Body의 Content Type (application/json) | O |

###### Response Body

| name | type | description | required |
|---|:---:|:---:|:---:|
| code | String | 사용자의 아이디 | O |
| message | String | 사용자의 비밀번호 | O |
| accessToken | String | 사용자의 아이디 | O |
| expires | int | 사용자의 비밀번호 | O |


###### Example

**응답 성공**
```bash
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
{
  "code": "SU",
  "message": "Success.",
  "accessToken": "${ACCESS_TOKEN}",
  "expires": 3600
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

**응답 : 실패 (로그인 정보 불일치)**
```bash
HTTP/1.1 401 Unauthorized
Content-Type: application/json;charset=UTF-8
{
  "code": "SF",
  "message": "Sign in Failed."
}
```

**응답 : 실패 (토큰 생성 실패)**
```bash
HTTP/1.1 500 Internal Server Error
Content-Type: application/json;charset=UTF-8
{
  "code": "TF",
  "message": "Token creation Failed."
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

#### - 아이디 찾기

##### 설명

클라이언트로부터 고객 | 디자이너는 회원가입시 등록한 이메일을 통해 인증을 받고 아이디를 찾을 수 잇다.
만약 잘못된 이메일, 인증번호를 불일치하게 되면 실패처리 됩니다. 데이터베이스 에러가 발생할 수 있습니다.

- method : **POST**  
- URL : **/id-found**  

##### Request

###### Header

| name | description | required |
|---|:---:|:---:|

###### Request Body

| name | type | description | required |
|---|:---:|:---:|:---:|
| userEmail | String | 사용자 이메일 (이메일 형태의 데이터) | O |
| authNumber | Int | 인증 확인할 인증 번호 | O |
| type | customer, desginer, admin |  

###### Example

```bash
curl -v -X POST "http://localhost:4200/api/v1/auth/id-found" \
 -d "userEmail=email@email.com" \
 -d "authNumber=0123"
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
| userId | String | 사용자아이디 | O |

###### Example

**응답 성공**
```bash
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
{
  "code": "SU",
  "message": "Success."
  "userId": "qwer1234"

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

**응답 : 실패 (중복된 이메일)**
```bash
HTTP/1.1 400 Bad Request
Content-Type: application/json;charset=UTF-8
{
  "code": "DE",
  "message": "Duplicatied Email."
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

#### - 비밀번호찾기 사용자 확인

##### 설명

클라이언트로부터 고객 | 디자이너는 회원가입시 등록한 아이디와 이메일을 통해 인증을 받고 비밀번호 재설정하는 페이지로 갈 수 있다.
만약 잘못된 아이디와 이메일, 이메일 인증번호 중 하나라도 입력이 잘못되면 실패처리 된다. 데이터베이스 에러가 발생할 수 있습니다.

- method : **POST**  
- URL : **/password-found**  

##### Request

###### Header

| name | description | required |
|---|:---:|:---:|

###### Request Body

| name | type | description | required |
|---|:---:|:---:|:---:|
| userId | String | 사용자 아이디 | O |
| userPassword | String | 사용자의 비밀번호 | O |
| userEmail | String | 사용자 이메일 (이메일 형태의 데이터) | O |
| authNumber | Int | 인증 확인할 인증 번호 | O |

###### Example

```bash
curl -v -X POST "http://localhost:4200/api/v1/auth/password_found" \
 -d "userId=service123" \
 -d "userEmail=email@email.com" \
 -d "authNumber=0123"
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

#### - 비밀번호 찾기 재설정
  
##### 설명
클라이언트로부터 재설정할 비밀번호를 필수로 입력하여 성공처리 한다. 만약 유효성에 어긋난다면 실패 처리한다. 데이터베이스 오류가 발생할 수 있다.
- method : **POST**  
- URL : **/password-reset**  

##### Request

###### Header

| name | description | required |
|---|:---:|:---:|

###### Request Body

| name | type | description | required |
|---|:---:|:---:|:---:|
| userPassword | String | 사용자 비밀번호 (영문+숫자 8~13자) | O |

###### Example

```bash
curl -v -X POST "http://localhost:4200/api/v1/auth/password-reset" \
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
