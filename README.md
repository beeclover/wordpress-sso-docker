## REF

https://www.keycloak.org/getting-started/getting-started-docker
https://plugins.miniorange.com/ko/keycloak-single-sign-on-wordpress-sso-oauth-openid-connect

wordpress 가이드는 keycloak의 구버전을 사용한 예시라서 이해하기 어려웠다. 줌라의 가이드를 보고 비슷하게 따라하면된다.
https://www.youtube.com/watch?v=8FJRHbKwquc

## keycloak + wordpress 튜토리얼

### keycloak 구성하기

#### 새로운 Realm을 생성
![](./docs/SCR-20231007-cggu.png)

Realm name에 test-wordpress 입력
![](./docs/SCR-20231007-chad.png)

#### 테스트용 Users 만들기
![](./docs/SCR-20231007-chob.png)
![](./docs/SCR-20231007-chua.png)

해당 유저로 들어가서 Credentials 생성하기

1. Users
2. 생성한 admin
3. Credentials 탭으로
4. 패스워드 생성
5. 패스워드 입력, Temporary 비활성화

![](./docs/SCR-20231007-ciij.png)

#### 테스트용 Client 만들기

![](./docs/SCR-20231007-cixi.png)

![](./docs/SCR-20231007-cjgm.png)

![](./docs/SCR-20231007-ckwf.png)

![](./docs/SCR-20231007-cjgt.png)

Client id와 Client secret 기억해두기, 복사해두기

![](./docs/SCR-20231007-clct.png)
![](./docs/SCR-20231007-clex.png)

### wordpress에서 SSO 활성화하기

wordpress plugins - `OAuth Single Sign On – SSO (OAuth Client)` 설치하기

![](./docs/SCR-20231007-cmif.png)
![](./docs/SCR-20231007-cmju.png)
![](./docs/SCR-20231007-cmlr.png)

3번에서 도메인을 아래처럼 설정한다.

Authorization Endpoint를 아래처럼

```diff
- {your-domain}/auth/realms/{realm}/protocol/openid-connect/auth
+ {your-domain}/realms/{realm}/protocol/openid-connect/auth
```

Token Endpoint를 아래처럼

```diff
- {your-domain}/realms/{realm}/protocol/openid-connect/token
+ {your-domain}/realms/{realm}/protocol/openid-connect/token
```

그리고 다음으로 넘어가면!


![](./docs/SCR-20231007-cvsa.png)

로그인완료!

## keycloak production

https://github.com/asatrya/keycloak-traefik-tutorial/blob/master/docker-compose.yml

keycloak production 모드가 아니라면 도메인을 설정하였을때 `/admin`으로 넘어갈때 iframe으로 인증절차를 하도록되어있는데 앱 내부에서 dev모드로 실행하면 `http`로 되어서 cors 오류가나게된다.

그래서 프로덕션모드로 실행하는 방법을 찾게되었다.