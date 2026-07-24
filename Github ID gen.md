# Github ID gen

# 1. Github 계정 생성

[https://github.com](https://github.com/)

우측 상단 Sign up을 통해

![image.png](Github%20ID%20gen/image.png)

개인정보들을 넣어줍니다. (메일주소는 기억해두시길 바랍니다.)

![image.png](Github%20ID%20gen/image%201.png)

축하드립니다. Github 계정을 방금 만드셨습니다.

# 2. Git - Github 연동

도대체 이 두개가 뭔 차이인지 가늠이 안갈수도 있는데 쉽게 생각하면 Github는 종착지고 Git은 버스입니다. 앞으로 작성될 코드에 대해서 git에 실어보내면 Github에도 적용된다고 보시면 됩니다.

### 1. Git 설치여부 확인

우선, 다들 Terminal을 실행하여

```bash
git --version
```

을 입력해봅니다.

만약 에러가 뜬다면,

```bash
sudo apt install git -y
```

를 입력하여 git을 설치해줍니다.

이후 다시 입력하면 정상적으로 git version이 출력될겁니다.

### 2. SSH 키 생성

터미널창에서

```bash
ssh-keygen -t ed25519 -C "아까 만든 깃허브 메일 주소"
eval "$(ssh-agent -s)"
```

ex(ssh-keygen -t ed25519 -C "miru@hanyang.ac.kr")

를 입력해줍니다. 이후

```bash
nano  ~/.ssh/config
```

를 입력하여 nano 창에 들어갑니다.

이후 파일의 최하단에

```bash
Host github.com
  AddKeysToAgent yes
  IgnoreUnknown UseKeychain
  IdentityFile ~/.ssh/id_ed25519
```

를 추가해줍니다. (Ctrl + X, Y 엔터 키 순차 입력을 통해 저장)

이후 마지막으로

```bash
ssh-add ~/.ssh/id_ed25519
```

를 입력하여 키를 추가해줍니다.

### 3. SSH 키 등록

이렇게 추가된 키를 서버에도 등록해주어야 하므로,

```bash
cat ~/.ssh/id_ed25519.pub
```

을 쳐서 나오는 내용을 복사해둡니다.

[https://github.com/settings/keys](https://github.com/settings/keys)

에 들어가서

![image.png](Github%20ID%20gen/image%202.png)

New SSH key를 고르신 다음

![image.png](Github%20ID%20gen/image%203.png)

Key에 아까 복사한 내용을 붙여넣어줍니다.

Title은 구분되기 편한 이름을 지어주시면 됩니다. (ex. Main laptop)

이후 Add SSH Key 까지 누르셨다면, 다시 Terminal 창에 돌아와서

```bash
ssh git@github.com
```

을 타이핑하시고 yes를 입력해주신다면, 성공적으로 연동되었음을 보여주는 창이 뜨게 됩니다.

![image.png](Github%20ID%20gen/image%204.png)