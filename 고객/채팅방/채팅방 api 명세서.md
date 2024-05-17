채팅방 api 명세서 0.4v
<h1 style='background-color: rgba(55, 55, 55, 0.4); text-align: center'>API 명세서</h1>

해당 API 명세서는 '(채팅방)'의 REST API를 명세하고 있습니다.

-Domain: http://localhost:4200

***

<h2 style='background-color: rgba(55, 55, 55, 0.2); text-align: center'>채팅방 모듈</h2>

인증 및 인가와 관련된 REST API 모듈
채팅방 개설, 메세지 보내기, 채팅방 나가기, 채팅방 목록 관리 등의 REST API가 포함되어 있습니다.

- url: /api/v1/


- 채팅방 개설
설명
고객이 디자이너와의 채팅방을 개설합니다.

method: POST
URL: /api/v1/chat-rooms

Request
Header
| name | description | required |
|---|:---:|:---:|
| Content-Type | application/json | O |
| Authorization | Bearer {access_token} | O |

Request Body
| name | type | description | required |
|---|:---:|:---:|:---:|
| designerId | String | 디자이너 ID | O |

Example
bash
curl -X POST "http://localhost:4200/api/v1/chat-rooms" \
-H "Content-Type: application/json" \
-H "Authorization: Bearer {access_token}" \
-d '{
  "designerId": "designer123"
}'

Response
Header
| name | description | required |
|---|:---:|:---:|
| Content-Type | application/json | O |

Response Body
| name | type | description | required |
|---|:---:|:---:|:---:|
| chatRoomId | String | 생성된 채팅방 ID | O |

Example
json
HTTP/1.1 201 Created
Content-Type: application/json
{
  "chatRoomId": "chatroom1"
}

- 채팅방 조회
설명
고객 또는 디자이너가 특정 채팅방을 조회합니다.

method: GET
URL: /api/v1/chat-rooms/{chatRoomId}

Request
Header
| name | description | required |
|---|:---:|:---:|
| Authorization | Bearer {access_token} | O |

Path Variable
| name | type | description | required |
|---|:---:|:---:|:---:|
| chatRoomId | String | 조회할 채팅방 ID | O |

Example
bash
curl -X GET "http://localhost:4200/api/v1/chat-rooms/chatroom1" \
-H "Authorization: Bearer {access_token}"

Response
Header
| name | description | required |
|---|:---:|:---:|
| Content-Type | application/json | O |

Response Body
| name | type | description | required |
|---|:---:|:---:|:---:|
| chatRoomId | String | 채팅방 ID | O |
| customerId | String | 고객 ID | O |
| designerId | String | 디자이너 ID | O |
| messages | Array | 메시지 목록 | O |
| messages[].messageId | String | 메시지 ID | O |
| messages[].senderId | String | 발신자 ID | O |
| messages[].content | String | 메시지 내용 | O |
| messages[].timestamp | String | 메시지 전송 시간 | O |

Example
json
HTTP/1.1 200 OK
Content-Type: application/json
{
  "chatRoomId": "chatroom1",
  "customerId": "customer123",
  "designerId": "designer123",
  "messages": [
    {
      "messageId": "message1",
      "senderId": "customer123",
      "content": "안녕하세요",
      "timestamp": "2023-06-01T10:00:00Z"
    },
    {
      "messageId": "message2",
      "senderId": "designer123",
      "content": "안녕하세요, 무엇을 도와드릴까요?",
      "timestamp": "2023-06-01T10:05:00Z"
    }
  ]
}

- 메시지 작성
설명
고객 또는 디자이너가 채팅방에 메시지를 작성합니다.

method: POST
URL: /api/v1/chat-rooms/{chatRoomId}/messages

Request
Header
| name | description | required |
|---|:---:|:---:|
| Content-Type | application/json | O |
| Authorization | Bearer {access_token} | O |

Path Variable
| name | type | description | required |
|---|:---:|:---:|:---:|
| chatRoomId | String | 메시지를 작성할 채팅방 ID | O |

Request Body
| name | type | description | required |
|---|:---:|:---:|:---:|
| content | String | 메시지 내용 | O |

Example
bash
curl -X POST "http://localhost:4200/api/v1/chat-rooms/chatroom1/messages" \
-H "Content-Type: application/json" \
-H "Authorization: Bearer {access_token}" \
-d '{
  "content": "새로운 메시지 내용"
}'
Response
Header
| name | description | required |
|---|:---:|:---:|
| Content-Type | application/json | O |

Response Body
| name | type | description | required |
|---|:---:|:---:|:---:|
| messageId | String | 생성된 메시지 ID | O |

Example
json
HTTP/1.1 201 Created
Content-Type: application/json
{
  "messageId": "message3"
}

- 메시지 신고
설명
고객 또는 디자이너가 상대방의 메시지를 신고합니다.

method: POST
URL: /api/v1/chat-rooms/{chatRoomId}/messages/{messageId}/report

Request
Header
| name | description | required |
|---|:---:|:---:|
| Content-Type | application/json | O |
| Authorization | Bearer {access_token} | O |

Path Variable
| name | type | description | required |
|---|:---:|:---:|:---:|
| chatRoomId | String | 신고할 메시지가 속한 채팅방 ID | O |
| messageId | String | 신고할 메시지 ID | O |

Request Body
| name | type | description | required |
|---|:---:|:---:|:---:|
| reason | String | 신고 사유 | O |

Example
bash
curl -X POST "http://localhost:4200/api/v1/chat-rooms/chatroom1/messages/message2/report" \
-H "Content-Type: application/json" \
-H "Authorization: Bearer {access_token}" \
-d '{
  "reason": "부적절한 내용"
}'

Response
Header
| name | description | required |
|---|:---:|:---:|
| Content-Type | application/json | O |

Response Body
| name | type | description | required |
|---|:---:|:---:|:---:|
| reportId | String | 생성된 신고 ID | O |

Example
json
HTTP/1.1 201 Created
Content-Type: application/json
{
  "reportId": "report1"
}

- 메시지 삭제
설명
고객 또는 디자이너가 자신이 작성한 메시지를 삭제합니다.

method: DELETE
URL: /api/v1/chat-rooms/{chatRoomId}/messages/{messageId}

Request
Header
| name | description | required |
|---|:---:|:---:|
| Authorization | Bearer {access_token} | O |

Path Variable
| name | type | description | required |
|---|:---:|:---:|:---:|
| chatRoomId | String | 삭제할 메시지가 속한 채팅방 ID | O |
| messageId | String | 삭제할 메시지 ID | O |

Example
bash
curl -X DELETE "http://localhost:4200/api/v1/chat-rooms/chatroom1/messages/message1" \
-H "Authorization: Bearer {access_token}"
Response
Example
HTTP/1.1 204 No Content

- 채팅방 나가기
설명
고객 또는 디자이너가 채팅방에서 나갑니다.

method: DELETE
URL: /api/v1/chat-rooms/{chatRoomId}/leave

Request
Header
| name | description | required |
|---|:---:|:---:|
| Authorization | Bearer {access_token} | O |

Path Variable
| name | type | description | required |
|---|:---:|:---:|:---:|
| chatRoomId | String | 나갈 채팅방 ID | O |

Example
bash
curl -X DELETE "http://localhost:4200/api/v1/chat-rooms/chatroom1/leave" \
-H "Authorization: Bearer {access_token}"

Response
Example
HTTP/1.1 204 No Content