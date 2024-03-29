# 启动

```
docker run -p 8080:8080 -e KEYCLOAK_ADMIN=admin -e KEYCLOAK_ADMIN_PASSWORD=admin quay.io/keycloak/keycloak:22.0.0 start-dev
```



# 概念



## Realm

## Realm OpenID Endpoint Configuration

http://sfq.me:8080/realms/demo/.well-known/openid-configuration

## User

## client









# Authorization Code  流程

https://datatracker.ietf.org/doc/html/rfc6749#section-4.1



## 登录 sfq-client 网站，发现没有 登录， 跳转 keycloak

```
http://sfq.me:8080/realms/demo/protocol/openid-connect/auth?client_id=sfq-client&redirect_uri=http%3A%2F%2Fsfq-client.com&state=12345678&response_mode=query&response_type=code&scope=openid
```



## 跳转 keycloak 登录以后，keycloak 跳转回来, 可以拿到授权码

```
http://sfq-client.com/?state=12345678&session_state=9fd8346d-e52f-4926-87d3-7da78003e821&code=8c0f9323-0d55-4449-ab41-9977de5a96df.9fd8346d-e52f-4926-87d3-7da78003e821.97d3de82-87c3-4d27-b58b-d77434995e81
```

## 

## 后端拿到授权码，使用 client_id 和 client_secret 和 code 获取 token

```
curl --location 'http://sfq.me:8080/realms/demo/protocol/openid-connect/token' \
--header 'Content-Type: application/x-www-form-urlencoded' \
--data-urlencode 'client_id=sfq-client' \
--data-urlencode 'grant_type=authorization_code' \
--data-urlencode 'code=8c0f9323-0d55-4449-ab41-9977de5a96df.9fd8346d-e52f-4926-87d3-7da78003e821.97d3de82-87c3-4d27-b58b-d77434995e81' \
--data-urlencode 'client_secret=LOPE1v34wooWotb6alvOyAAirIIMejUg' \
--data-urlencode 'redirect_uri=http://sfq-client.com'
```



```json
{
    "access_token": "eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICJ5NGFHb3JZR2dYS3VDdkZpTXFQU0FodjdRRUdxektuckdjdHZ2d25JWW1ZIn0.eyJleHAiOjE2ODk0MDU1NzQsImlhdCI6MTY4OTQwNTI3NCwiYXV0aF90aW1lIjoxNjg5NDA0OTMwLCJqdGkiOiIzYjFjMGIxYS1mZGE4LTQ1YWYtYTFkMC05M2YyYmZkMmFhYTUiLCJpc3MiOiJodHRwOi8vc2ZxLm1lOjgwODAvcmVhbG1zL2RlbW8iLCJhdWQiOiJhY2NvdW50Iiwic3ViIjoiMWFjZTQzNTQtYTQ4Ny00NTA2LTljODUtMzhmNTk3OTE5Yjg0IiwidHlwIjoiQmVhcmVyIiwiYXpwIjoic2ZxLWNsaWVudCIsInNlc3Npb25fc3RhdGUiOiI5ZmQ4MzQ2ZC1lNTJmLTQ5MjYtODdkMy03ZGE3ODAwM2U4MjEiLCJhY3IiOiIwIiwiYWxsb3dlZC1vcmlnaW5zIjpbIiIsIioiXSwicmVhbG1fYWNjZXNzIjp7InJvbGVzIjpbIm9mZmxpbmVfYWNjZXNzIiwiZGVmYXVsdC1yb2xlcy1kZW1vIiwidW1hX2F1dGhvcml6YXRpb24iXX0sInJlc291cmNlX2FjY2VzcyI6eyJhY2NvdW50Ijp7InJvbGVzIjpbIm1hbmFnZS1hY2NvdW50IiwibWFuYWdlLWFjY291bnQtbGlua3MiLCJ2aWV3LXByb2ZpbGUiXX19LCJzY29wZSI6Im9wZW5pZCBhZGRyZXNzIGVtYWlsIG9mZmxpbmVfYWNjZXNzIHBob25lIHByb2ZpbGUgbWljcm9wcm9maWxlLWp3dCIsInNpZCI6IjlmZDgzNDZkLWU1MmYtNDkyNi04N2QzLTdkYTc4MDAzZTgyMSIsInVwbiI6InNmcUBnbWFpbC5jb20iLCJlbWFpbF92ZXJpZmllZCI6dHJ1ZSwiYWRkcmVzcyI6e30sIm5hbWUiOiJmcSBzIiwiZ3JvdXBzIjpbIm9mZmxpbmVfYWNjZXNzIiwiZGVmYXVsdC1yb2xlcy1kZW1vIiwidW1hX2F1dGhvcml6YXRpb24iXSwicHJlZmVycmVkX3VzZXJuYW1lIjoic2ZxQGdtYWlsLmNvbSIsImdpdmVuX25hbWUiOiJmcSIsImZhbWlseV9uYW1lIjoicyIsImVtYWlsIjoic2ZxQGdtYWlsLmNvbSJ9.ap27xZR_a_-FIeAS0xLi_o56KBVFms5hCHpLIot-sihEhS8u2XuONrGydsIgVNo4gNPFwbbRtqXuRA5sCNTHVOUTv1Ldat55IhkfHMSvTqFxLJko3tUiGdnbDh7lzLgAUeLw8buMzw3o-SQLRuEfMD0nV2d7jY2Xpp2I9jum2UQRNfTm6cOT_w6ZL4TArjXC38OJTDqd-Wn_qtjI8KtvZIRTwSuw5nTGPe6CRbiuzFsN1n4Q7nK3rifERrFK94hYhLSid82GPu7zUn9LzmUDM6snRScfAxKOfJ4veCZfcW1eO0J5NmvFx2Z5NcOtAKiK6YvvNOD5axN7LjC_zkmwNw",
    "expires_in": 300,
    "refresh_expires_in": 0,
    "refresh_token": "eyJhbGciOiJIUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICI2ZjFlYTQwMC1lNDIwLTQ3YjAtODJiYS0xY2Q2MTUwOWUwZjUifQ.eyJpYXQiOjE2ODk0MDUyNzQsImp0aSI6IjNkMzM1NGJlLTY0Y2EtNDczYS05ZjkxLWMxNWZkOGNjOTI3YiIsImlzcyI6Imh0dHA6Ly9zZnEubWU6ODA4MC9yZWFsbXMvZGVtbyIsImF1ZCI6Imh0dHA6Ly9zZnEubWU6ODA4MC9yZWFsbXMvZGVtbyIsInN1YiI6IjFhY2U0MzU0LWE0ODctNDUwNi05Yzg1LTM4ZjU5NzkxOWI4NCIsInR5cCI6Ik9mZmxpbmUiLCJhenAiOiJzZnEtY2xpZW50Iiwic2Vzc2lvbl9zdGF0ZSI6IjlmZDgzNDZkLWU1MmYtNDkyNi04N2QzLTdkYTc4MDAzZTgyMSIsInNjb3BlIjoib3BlbmlkIGFkZHJlc3MgZW1haWwgb2ZmbGluZV9hY2Nlc3MgcGhvbmUgcHJvZmlsZSBtaWNyb3Byb2ZpbGUtand0Iiwic2lkIjoiOWZkODM0NmQtZTUyZi00OTI2LTg3ZDMtN2RhNzgwMDNlODIxIn0.2r_VUOoCdidK7jGzCQB-kOCi5n1UVlnsqd1syPC9tzQ",
    "token_type": "Bearer",
    "id_token": "eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICJ5NGFHb3JZR2dYS3VDdkZpTXFQU0FodjdRRUdxektuckdjdHZ2d25JWW1ZIn0.eyJleHAiOjE2ODk0MDU1NzQsImlhdCI6MTY4OTQwNTI3NCwiYXV0aF90aW1lIjoxNjg5NDA0OTMwLCJqdGkiOiIzZWJlYzNkNC00MWI2LTQwYTMtYWUwYi1lZDE5ODRkZTk0MGEiLCJpc3MiOiJodHRwOi8vc2ZxLm1lOjgwODAvcmVhbG1zL2RlbW8iLCJhdWQiOiJzZnEtY2xpZW50Iiwic3ViIjoiMWFjZTQzNTQtYTQ4Ny00NTA2LTljODUtMzhmNTk3OTE5Yjg0IiwidHlwIjoiSUQiLCJhenAiOiJzZnEtY2xpZW50Iiwic2Vzc2lvbl9zdGF0ZSI6IjlmZDgzNDZkLWU1MmYtNDkyNi04N2QzLTdkYTc4MDAzZTgyMSIsImF0X2hhc2giOiJIWnFqRUVsUk9JM1U2YVB4RDlsaTd3IiwiYWNyIjoiMCIsInNpZCI6IjlmZDgzNDZkLWU1MmYtNDkyNi04N2QzLTdkYTc4MDAzZTgyMSIsInVwbiI6InNmcUBnbWFpbC5jb20iLCJlbWFpbF92ZXJpZmllZCI6dHJ1ZSwiYWRkcmVzcyI6e30sIm5hbWUiOiJmcSBzIiwiZ3JvdXBzIjpbIm9mZmxpbmVfYWNjZXNzIiwiZGVmYXVsdC1yb2xlcy1kZW1vIiwidW1hX2F1dGhvcml6YXRpb24iXSwicHJlZmVycmVkX3VzZXJuYW1lIjoic2ZxQGdtYWlsLmNvbSIsImdpdmVuX25hbWUiOiJmcSIsImZhbWlseV9uYW1lIjoicyIsImVtYWlsIjoic2ZxQGdtYWlsLmNvbSJ9.VRimAvqjgBgDX4j69HqBSxUET16JVgTlT5FU9p-sNd3DehUCFQiR2dmUZSMN3w7SoKiotMBd73uz0xlHSWX8dj5EcJrIz9s7AtpHloBG2MFGqA2BowvQWgh7VTmsj1EItNILtzdBTy6D7sYDJNDn5xubtFPwi50wAqxQWm_B465jmlkeZ6ybH9FYIUB3hPbMRaIk_duuPF1W68hLS5J5STf0oS5Nk_4pTrL-mRXXe5RwqVO9IBw_a6_BkcZ3GMbWjKNPb8jyF-IRZpImSIhFJHo276mZTJlhljzqW8lgS5zzJHDtL44s7EhZGnXmlYJEPGeRtu1YKxJX0woyyik3uw",
    "not-before-policy": 0,
    "session_state": "9fd8346d-e52f-4926-87d3-7da78003e821",
    "scope": "openid address email offline_access phone profile microprofile-jwt"
}
```



# implict 流程

```
http://sfq.me:8080/realms/demo/protocol/openid-connect/auth?client_id=sfq-client&redirect_uri=http%3A%2F%2Fsfq-client.com&state=12345678&response_type=token&scope=openid

http://sfq-client.com/#state=12345678&session_state=9fd8346d-e52f-4926-87d3-7da78003e821&access_token=eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICJ5NGFHb3JZR2dYS3VDdkZpTXFQU0FodjdRRUdxektuckdjdHZ2d25JWW1ZIn0.eyJleHAiOjE2ODk0MDcxNjIsImlhdCI6MTY4OTQwNjI2MiwiYXV0aF90aW1lIjoxNjg5NDA0OTMwLCJqdGkiOiIwNWYwMDFjMi0xN2U0LTQzODEtYWJiNS1iMDBjZTE3NWM1Y2YiLCJpc3MiOiJodHRwOi8vc2ZxLm1lOjgwODAvcmVhbG1zL2RlbW8iLCJhdWQiOiJhY2NvdW50Iiwic3ViIjoiMWFjZTQzNTQtYTQ4Ny00NTA2LTljODUtMzhmNTk3OTE5Yjg0IiwidHlwIjoiQmVhcmVyIiwiYXpwIjoic2ZxLWNsaWVudCIsInNlc3Npb25fc3RhdGUiOiI5ZmQ4MzQ2ZC1lNTJmLTQ5MjYtODdkMy03ZGE3ODAwM2U4MjEiLCJhY3IiOiIwIiwiYWxsb3dlZC1vcmlnaW5zIjpbIiIsIioiXSwicmVhbG1fYWNjZXNzIjp7InJvbGVzIjpbIm9mZmxpbmVfYWNjZXNzIiwiZGVmYXVsdC1yb2xlcy1kZW1vIiwidW1hX2F1dGhvcml6YXRpb24iXX0sInJlc291cmNlX2FjY2VzcyI6eyJhY2NvdW50Ijp7InJvbGVzIjpbIm1hbmFnZS1hY2NvdW50IiwibWFuYWdlLWFjY291bnQtbGlua3MiLCJ2aWV3LXByb2ZpbGUiXX19LCJzY29wZSI6Im9wZW5pZCBhZGRyZXNzIGVtYWlsIG9mZmxpbmVfYWNjZXNzIHBob25lIHByb2ZpbGUgbWljcm9wcm9maWxlLWp3dCIsInNpZCI6IjlmZDgzNDZkLWU1MmYtNDkyNi04N2QzLTdkYTc4MDAzZTgyMSIsInVwbiI6InNmcUBnbWFpbC5jb20iLCJlbWFpbF92ZXJpZmllZCI6dHJ1ZSwiYWRkcmVzcyI6e30sIm5hbWUiOiJmcSBzIiwiZ3JvdXBzIjpbIm9mZmxpbmVfYWNjZXNzIiwiZGVmYXVsdC1yb2xlcy1kZW1vIiwidW1hX2F1dGhvcml6YXRpb24iXSwicHJlZmVycmVkX3VzZXJuYW1lIjoic2ZxQGdtYWlsLmNvbSIsImdpdmVuX25hbWUiOiJmcSIsImZhbWlseV9uYW1lIjoicyIsImVtYWlsIjoic2ZxQGdtYWlsLmNvbSJ9.iDdUjFolDv6Zbut0Oe2IyAE24yLhfaDI1ncgxQiOwFghkA59w1-3gHDgCaJzp2l2CI0Ig3hj2BXNGz9jKLDM6L28A43sr_Flc4AFkk0tz35LthdZxeNVVgyBSIxyeDLTNg70P5KY4VIF_rCsJUrb6pTHe3zPneImHJuUtzayGiehbPaE0hDsgOZfsF8pTTS4_OUpgMRhwlMujl5Px7j0taLWDwDhMl20Je0jF7rz1xOc7dLOsvlMEo2dNRDzA-ZgVqySpr4Uix7HytL3L0SfP5EJYOHtwgXXwhz8TViAAU6CRpsQqpBCxRN4ba0ok4h4tHS1vKXuIiYpeaYh_LljaQ&token_type=Bearer&expires_in=900
```



# Client Credentials Grant 流程

```
curl --location 'http://sfq.me:8080/realms/demo/protocol/openid-connect/token' \
--header 'Content-Type: application/x-www-form-urlencoded' \
--data-urlencode 'client_id=sfq-client' \
--data-urlencode 'client_secret=LOPE1v34wooWotb6alvOyAAirIIMejUg' \
--data-urlencode 'grant_type=client_credentials'


{
    "access_token": "eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICJ5NGFHb3JZR2dYS3VDdkZpTXFQU0FodjdRRUdxektuckdjdHZ2d25JWW1ZIn0.eyJleHAiOjE2ODk0MDkxMzUsImlhdCI6MTY4OTQwODgzNSwianRpIjoiNmVmMTJkMWMtOGVhMy00MzMyLTk2YmItODQ2YWIxZWYzYTQwIiwiaXNzIjoiaHR0cDovL3NmcS5tZTo4MDgwL3JlYWxtcy9kZW1vIiwiYXVkIjoiYWNjb3VudCIsInN1YiI6Ijg4MGIwZDUyLWZiNDAtNDk5MC1iMzA2LTI2MGEwYTg2MTJhZSIsInR5cCI6IkJlYXJlciIsImF6cCI6InNmcS1jbGllbnQiLCJhY3IiOiIxIiwiYWxsb3dlZC1vcmlnaW5zIjpbIiIsIioiXSwicmVhbG1fYWNjZXNzIjp7InJvbGVzIjpbIm9mZmxpbmVfYWNjZXNzIiwiZGVmYXVsdC1yb2xlcy1kZW1vIiwidW1hX2F1dGhvcml6YXRpb24iXX0sInJlc291cmNlX2FjY2VzcyI6eyJzZnEtY2xpZW50Ijp7InJvbGVzIjpbInVtYV9wcm90ZWN0aW9uIl19LCJhY2NvdW50Ijp7InJvbGVzIjpbIm1hbmFnZS1hY2NvdW50IiwibWFuYWdlLWFjY291bnQtbGlua3MiLCJ2aWV3LXByb2ZpbGUiXX19LCJzY29wZSI6ImFkZHJlc3MgZW1haWwgb2ZmbGluZV9hY2Nlc3MgcGhvbmUgcHJvZmlsZSBtaWNyb3Byb2ZpbGUtand0IiwidXBuIjoic2VydmljZS1hY2NvdW50LXNmcS1jbGllbnQiLCJlbWFpbF92ZXJpZmllZCI6ZmFsc2UsImNsaWVudEhvc3QiOiIxNzIuMTcuMC4xIiwiYWRkcmVzcyI6e30sImdyb3VwcyI6WyJvZmZsaW5lX2FjY2VzcyIsImRlZmF1bHQtcm9sZXMtZGVtbyIsInVtYV9hdXRob3JpemF0aW9uIl0sInByZWZlcnJlZF91c2VybmFtZSI6InNlcnZpY2UtYWNjb3VudC1zZnEtY2xpZW50IiwiY2xpZW50QWRkcmVzcyI6IjE3Mi4xNy4wLjEiLCJjbGllbnRfaWQiOiJzZnEtY2xpZW50In0.CkuetdO1H8ibJSdrl5TKr9L2pCfIxfK7x_hVMTgG0pK7QbPBxPaKrGA__-KfCRMDSSZ9MwVcDnK-ijCYTNMRskXQ78iUlPrt0poHlz1YyjK9YSiwSbk2DrY1ijSxq1ZSLt7OlqPAK29Fqw38URVT1Onojtysfwd0iuK2uts4qwXac8rFn2qQF4iy9Y20vxsSb7nyBF0YiHcyFhVMm1gaNDEnHfjSRBjWyg3ATtZV9d2XrhMlL5ENxVyv62KJRmXaIiSEqRrTFz1gnh7iQ14VGmRp5LlC5gmGuzqOE_lSr25vuF4uNXQkvULfBI96-Ro2ZvqL9uIH8wgrX8JrjdyUTQ",
    "expires_in": 300,
    "refresh_expires_in": 0,
    "token_type": "Bearer",
    "not-before-policy": 0,
    "scope": "address email offline_access phone profile microprofile-jwt"
}
```



# Resource Owner Password Credentials Grant 流程

```
curl --location 'http://sfq.me:8080/realms/demo/protocol/openid-connect/token' \
--header 'Content-Type: application/x-www-form-urlencoded' \
--data-urlencode 'grant_type=password' \
--data-urlencode 'username=sfq@gmail.com' \
--data-urlencode 'password=123456' \
--data-urlencode 'client_id=sfq-client' \
--data-urlencode 'client_secret=LOPE1v34wooWotb6alvOyAAirIIMejUg'


{
    "access_token": "eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICJ5NGFHb3JZR2dYS3VDdkZpTXFQU0FodjdRRUdxektuckdjdHZ2d25JWW1ZIn0.eyJleHAiOjE2ODk0MDc5MzgsImlhdCI6MTY4OTQwNzYzOCwianRpIjoiMmRjNWQ5NWYtM2ZkYi00YjQzLTk3YjEtNTk2YzZlNTU4MTBjIiwiaXNzIjoiaHR0cDovL3NmcS5tZTo4MDgwL3JlYWxtcy9kZW1vIiwiYXVkIjoiYWNjb3VudCIsInN1YiI6IjFhY2U0MzU0LWE0ODctNDUwNi05Yzg1LTM4ZjU5NzkxOWI4NCIsInR5cCI6IkJlYXJlciIsImF6cCI6InNmcS1jbGllbnQiLCJzZXNzaW9uX3N0YXRlIjoiNzg3NmIzOTItMGVmYS00NTQ5LWE5ZmItYTI5YzA2NmZlY2U0IiwiYWNyIjoiMSIsImFsbG93ZWQtb3JpZ2lucyI6WyIiLCIqIl0sInJlYWxtX2FjY2VzcyI6eyJyb2xlcyI6WyJvZmZsaW5lX2FjY2VzcyIsImRlZmF1bHQtcm9sZXMtZGVtbyIsInVtYV9hdXRob3JpemF0aW9uIl19LCJyZXNvdXJjZV9hY2Nlc3MiOnsiYWNjb3VudCI6eyJyb2xlcyI6WyJtYW5hZ2UtYWNjb3VudCIsIm1hbmFnZS1hY2NvdW50LWxpbmtzIiwidmlldy1wcm9maWxlIl19fSwic2NvcGUiOiJhZGRyZXNzIGVtYWlsIG9mZmxpbmVfYWNjZXNzIHBob25lIHByb2ZpbGUgbWljcm9wcm9maWxlLWp3dCIsInNpZCI6Ijc4NzZiMzkyLTBlZmEtNDU0OS1hOWZiLWEyOWMwNjZmZWNlNCIsInVwbiI6InNmcUBnbWFpbC5jb20iLCJlbWFpbF92ZXJpZmllZCI6dHJ1ZSwiYWRkcmVzcyI6e30sIm5hbWUiOiJmcSBzIiwiZ3JvdXBzIjpbIm9mZmxpbmVfYWNjZXNzIiwiZGVmYXVsdC1yb2xlcy1kZW1vIiwidW1hX2F1dGhvcml6YXRpb24iXSwicHJlZmVycmVkX3VzZXJuYW1lIjoic2ZxQGdtYWlsLmNvbSIsImdpdmVuX25hbWUiOiJmcSIsImZhbWlseV9uYW1lIjoicyIsImVtYWlsIjoic2ZxQGdtYWlsLmNvbSJ9.pD2ipsfNiFtktEQu5wd8FnD48xc9oTT5Amxa0UcI0pDSUknLgjUQPZZIjKHBqduX4aRFAlf4rIvhxazyzu64BiRtxQDuo8_0war19xts7RsQMcCO88pMDaOGGJGYWcS9EQZ8yOibnOyfu-WDNF5zGwDRDkpz9DomNhdrAto62W-J7FOb_6HjvYTD_X-Xb1_VK232lW_Pz9EyWLZI2ug-znE-W7VkfEKnboniJslWEh3nmaHGK-bGupeqDuGZQ6fxaXwkxamj7Jlkba04xv5V9hp8z4NU6I1YQzb5dscsQiLgroiMG7Mfi4NNoIbtnLFpNM72-cFwXgVMXH-dVwVshg",
    "expires_in": 300,
    "refresh_expires_in": 0,
    "refresh_token": "eyJhbGciOiJIUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICI2ZjFlYTQwMC1lNDIwLTQ3YjAtODJiYS0xY2Q2MTUwOWUwZjUifQ.eyJpYXQiOjE2ODk0MDc2MzgsImp0aSI6IjMxNjI0NmY0LWUyY2ItNDhiYi1iZjE0LTBlOGViYTI1ZTc0MCIsImlzcyI6Imh0dHA6Ly9zZnEubWU6ODA4MC9yZWFsbXMvZGVtbyIsImF1ZCI6Imh0dHA6Ly9zZnEubWU6ODA4MC9yZWFsbXMvZGVtbyIsInN1YiI6IjFhY2U0MzU0LWE0ODctNDUwNi05Yzg1LTM4ZjU5NzkxOWI4NCIsInR5cCI6Ik9mZmxpbmUiLCJhenAiOiJzZnEtY2xpZW50Iiwic2Vzc2lvbl9zdGF0ZSI6Ijc4NzZiMzkyLTBlZmEtNDU0OS1hOWZiLWEyOWMwNjZmZWNlNCIsInNjb3BlIjoiYWRkcmVzcyBlbWFpbCBvZmZsaW5lX2FjY2VzcyBwaG9uZSBwcm9maWxlIG1pY3JvcHJvZmlsZS1qd3QiLCJzaWQiOiI3ODc2YjM5Mi0wZWZhLTQ1NDktYTlmYi1hMjljMDY2ZmVjZTQifQ.dzS463k-m26pAMVsHyrVeIuRHWjXROeet1QUobzEOfQ",
    "token_type": "Bearer",
    "not-before-policy": 0,
    "session_state": "7876b392-0efa-4549-a9fb-a29c066fece4",
    "scope": "address email offline_access phone profile microprofile-jwt"
}
```



# 其他说明



## "Confidential" access type

"Confidential" access type通常是指一种安全访问模式，用于在客户端和服务器之间建立安全通信。在这种访问模式下，客户端必须提供一些凭据（如身份验证令牌或用户名和密码），以证明其身份和权限。这些凭据可以使用加密或其他安全机制进行保护，以确保只有授权的用户或客户端可以访问受保护的资源。

在 OAuth2.0 授权流程中，"Confidential" access type通常用于表示安全客户端，如 Web 应用程序或后端服务，它们可以安全地保持客户端凭据（如客户端 ID 和客户端秘钥）的机密性，以便安全地访问受保护的资源。与之相对的是"Public" access type，它通常用于表示不安全客户端，如移动应用或浏览器应用程序，它们无法安全地保持客户端凭据的机密性，因此需要使用其他授权流程，如授权码授权流程或隐式授权流程来保护资源的安全性。

总之，"Confidential" access type是一种安全的访问模式，可以帮助确保只有授权的用户或客户端可以访问受保护的资源，并提供了一些安全机制来保护客户端凭据的机密性和安全性。



## Client Credentials

客户端和资源服务器之间建立机器到机器的身份验证和授权。它通常用于后端服务或机器人应用程序，这些应用程序需要在没有用户交互的情况下访问受保护的资源。

由于它不需要用户交互或资源所有者的身份验证，因此可以提供更快速和可靠的身份验证方式。此外，由于客户端凭据是预先分配的，因此客户端可以在没有用户参与的情况下向授权服务器进行身份验证和授权，从而提高了应用程序的用户体验和响应速度。

然而，由于 Client Credentials OAuth2.0 授权模式是一种机器到机器的身份验证方式，因此需要特别注意安全性问题。例如，如果客户端凭据泄露，可能会导致未经授权的应用程序访问受保护的资源。因此，在使用 Client Credentials OAuth2.0 授权模式时，需要采取适当的安全措施来保护客户端凭据和访问令牌的机密性和安全性。

## Resource Owner Password Credentials

由于它不需要通过授权服务器来获取访问令牌，而是直接使用用户名和密码来进行身份验证，因此它可以在某些情况下提供更加便捷的身份验证方式。例如，在一些受限的环境中，客户端可能无法使用其他 OAuth2.0 授权模式，如授权码或隐式授权流程，而 ROPC 授权模式则可以提供一种简单而有效的身份验证方式。

此外，ROPC 授权模式还可以提供更加灵活的身份验证方式。例如，资源所有者可以使用不同的身份验证方式，如密码、生物识别信息或多因素身份验证等。这些身份验证方式可以在授权服务器上进行配置，并根据客户端的要求进行定制。

然而，ROPC 授权模式也存在一些安全风险。由于客户端需要知道资源所有者的密码，因此如果客户端存储了密码或通过不安全的方式传输密码，则可能会导致密码泄露的风险。因此，在使用 ROPC 授权模式时，需要确保客户端的安全性，并采取适当的措施来保护资源所有者的密码和访问令牌。

