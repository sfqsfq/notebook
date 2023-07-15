```
# 获取 token

curl --location 'http://sfq.me:8080/realms/demo/protocol/openid-connect/token' \
--header 'Content-Type: application/x-www-form-urlencoded' \
--data-urlencode 'username=admin@gmail.com' \
--data-urlencode 'password=admin' \
--data-urlencode 'grant_type=password' \
--data-urlencode 'client_id=admin-cli' \
--data-urlencode 'client_secret=lFufJ2JII10pjsin1z45yHq4oIcPQOgq'

# 返回结果
{
    "access_token": "eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICJ5NGFHb3JZR2dYS3VDdkZpTXFQU0FodjdRRUdxektuckdjdHZ2d25JWW1ZIn0.eyJleHAiOjE2ODk0Mjc0NjIsImlhdCI6MTY4OTQyNzE2MiwianRpIjoiNDQ1NTM5ZjQtMTY5ZS00Y2U1LTgwNDQtNmRjYzNjNTk1NDY3IiwiaXNzIjoiaHR0cDovL3NmcS5tZTo4MDgwL3JlYWxtcy9kZW1vIiwic3ViIjoiMjI3YzQyYTctMjdmMy00ZmNhLWFhOWUtMWU1NTUxMzcxZGE1IiwidHlwIjoiQmVhcmVyIiwiYXpwIjoiYWRtaW4tY2xpIiwic2Vzc2lvbl9zdGF0ZSI6ImExZTNiODUxLTI0NjMtNDgyNi04NTEwLWEwNTMxNjUwYjQzNCIsImFjciI6IjEiLCJzY29wZSI6ImVtYWlsIHByb2ZpbGUiLCJzaWQiOiJhMWUzYjg1MS0yNDYzLTQ4MjYtODUxMC1hMDUzMTY1MGI0MzQiLCJlbWFpbF92ZXJpZmllZCI6dHJ1ZSwicHJlZmVycmVkX3VzZXJuYW1lIjoiYWRtaW5AZ21haWwuY29tIiwiZ2l2ZW5fbmFtZSI6IiIsImZhbWlseV9uYW1lIjoiIiwiZW1haWwiOiJhZG1pbkBnbWFpbC5jb20ifQ.oqy40WKd4TW3RtzO078fx63xT3jY2975v02iOLBRhic1d01f7uG8ZZrpFOBlYos7t-ndekiRKUZKnIayeAui87_PZOAb0MnJsSwxfkMOVzXPh2PFYngjhfv25QKQaHHuoN7ZywNfnzbPNnDeDBJHE-sJPjV-PADZtahUtxlJzP9qJZWI2Yd3LuBD2ALGxFGXlhFZJY0W2vihPMYDSytxY5jPtKGffWRBElxLfP7Ha62QzMdmDVoVHD7jJRjix9ahbP54OrsF_a_Sb38gl9YlO2OLcx9QyWIBll1pydf1aizc1vsLP_B7L0oxMkAvnnbI5_6qsztRmqqUyxv12qMbUw",
    "expires_in": 300,
    "refresh_expires_in": 1800,
    "refresh_token": "eyJhbGciOiJIUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICI2ZjFlYTQwMC1lNDIwLTQ3YjAtODJiYS0xY2Q2MTUwOWUwZjUifQ.eyJleHAiOjE2ODk0Mjg5NjIsImlhdCI6MTY4OTQyNzE2MiwianRpIjoiMGYxODcwYmYtMDc0Yy00OGYwLWE2OGQtOWRiZTIxNjhkZGZjIiwiaXNzIjoiaHR0cDovL3NmcS5tZTo4MDgwL3JlYWxtcy9kZW1vIiwiYXVkIjoiaHR0cDovL3NmcS5tZTo4MDgwL3JlYWxtcy9kZW1vIiwic3ViIjoiMjI3YzQyYTctMjdmMy00ZmNhLWFhOWUtMWU1NTUxMzcxZGE1IiwidHlwIjoiUmVmcmVzaCIsImF6cCI6ImFkbWluLWNsaSIsInNlc3Npb25fc3RhdGUiOiJhMWUzYjg1MS0yNDYzLTQ4MjYtODUxMC1hMDUzMTY1MGI0MzQiLCJzY29wZSI6ImVtYWlsIHByb2ZpbGUiLCJzaWQiOiJhMWUzYjg1MS0yNDYzLTQ4MjYtODUxMC1hMDUzMTY1MGI0MzQifQ.fdrbSRIlf2c6XGzQfVecXoBpq7W8LSjl9gRRZQGphs4",
    "token_type": "Bearer",
    "not-before-policy": 0,
    "session_state": "a1e3b851-2463-4826-8510-a0531650b434",
    "scope": "email profile"
}


# 获取用户信息
curl --location 'http://sfq.me:8080/admin/realms/demo/users?email=sfq%40gmail.com' \
--header 'Authorization: Bearer eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICJ5NGFHb3JZR2dYS3VDdkZpTXFQU0FodjdRRUdxektuckdjdHZ2d25JWW1ZIn0.eyJleHAiOjE2ODk0Mjc0NjIsImlhdCI6MTY4OTQyNzE2MiwianRpIjoiNDQ1NTM5ZjQtMTY5ZS00Y2U1LTgwNDQtNmRjYzNjNTk1NDY3IiwiaXNzIjoiaHR0cDovL3NmcS5tZTo4MDgwL3JlYWxtcy9kZW1vIiwic3ViIjoiMjI3YzQyYTctMjdmMy00ZmNhLWFhOWUtMWU1NTUxMzcxZGE1IiwidHlwIjoiQmVhcmVyIiwiYXpwIjoiYWRtaW4tY2xpIiwic2Vzc2lvbl9zdGF0ZSI6ImExZTNiODUxLTI0NjMtNDgyNi04NTEwLWEwNTMxNjUwYjQzNCIsImFjciI6IjEiLCJzY29wZSI6ImVtYWlsIHByb2ZpbGUiLCJzaWQiOiJhMWUzYjg1MS0yNDYzLTQ4MjYtODUxMC1hMDUzMTY1MGI0MzQiLCJlbWFpbF92ZXJpZmllZCI6dHJ1ZSwicHJlZmVycmVkX3VzZXJuYW1lIjoiYWRtaW5AZ21haWwuY29tIiwiZ2l2ZW5fbmFtZSI6IiIsImZhbWlseV9uYW1lIjoiIiwiZW1haWwiOiJhZG1pbkBnbWFpbC5jb20ifQ.oqy40WKd4TW3RtzO078fx63xT3jY2975v02iOLBRhic1d01f7uG8ZZrpFOBlYos7t-ndekiRKUZKnIayeAui87_PZOAb0MnJsSwxfkMOVzXPh2PFYngjhfv25QKQaHHuoN7ZywNfnzbPNnDeDBJHE-sJPjV-PADZtahUtxlJzP9qJZWI2Yd3LuBD2ALGxFGXlhFZJY0W2vihPMYDSytxY5jPtKGffWRBElxLfP7Ha62QzMdmDVoVHD7jJRjix9ahbP54OrsF_a_Sb38gl9YlO2OLcx9QyWIBll1pydf1aizc1vsLP_B7L0oxMkAvnnbI5_6qsztRmqqUyxv12qMbUw'

# 返回结果
[
    {
        "id": "1ace4354-a487-4506-9c85-38f597919b84",
        "createdTimestamp": 1689390544699,
        "username": "sfq@gmail.com",
        "enabled": true,
        "totp": false,
        "emailVerified": true,
        "firstName": "fq",
        "lastName": "s",
        "email": "sfq@gmail.com",
        "attributes": {
            "attribute_key_1": [
                "attribute_value_1"
            ]
        },
        "disableableCredentialTypes": [],
        "requiredActions": [],
        "notBefore": 0,
        "access": {
            "manageGroupMembership": true,
            "view": true,
            "mapRoles": true,
            "impersonate": true,
            "manage": true
        }
    }
]
```



[其他接口](https://www.keycloak.org/docs-api/18.0/rest-api/#_overview)
