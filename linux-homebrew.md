Homebrew 대해
---------------
Homebrew의 어원은 맥주를 스스로 양조해서 마신다는 뜻으로, 사용자가 스스로 패키지를 빌드하고 관리하게 해준다는 의미로 네이밍을 한 것 같다. 즉, Mac에서 사용자가 소프트웨어를 도입하는데 최소한의 비용을 들이고 이를 버전별로 관리해 주는 패키지 관리시스템 중에 하나라고 보면 된다. yum, apt-get 등과 비슷하다.

### 용어 정의
|   용어    |    원 의미            |     재해석                |
| :------- |:-------------------:|:------------------------ |
| Brew     | 양조                 | 소프트웨어 Build            |
| HomeBrew | 자가 양조             | 소프트웨어 자가 구축          |
| Celler   | 와인이나 맥주 등의 저장고 | 소프트웨어 설치 장소          |
| Keg      | 통                  | Build 자료               |
| Formula  | 단계별 요리법          | 빌드 방법/절차가 작성된 스크립트 |

Homebrew 설치
---------------
### 1. 사전 필요한 소프트웨어
- [cURL](https://curl.haxx.se/): `sudo apt install curl`

### 2. Homebrew 설치
아래 커맨드를 실행하면 자동으로 설치된다.
```
$ /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
$ brew doctor # 문제 진단을 해줌.
$ brew update # 최신버전 업데이트.
```

### 3. 기타
brew search 시에 만나게 되는 "GitHub API rate limit exceeded" 메세지 출현을 해결하기 이해 아래 방법대로 처리한다.
```
# 남은 조회 횟수 확인
$ curl -i https://api.github.com/users/whatever
# 그리고 https://github.com/settings/tokens에서 Generate new token버튼 클릭해서 토큰 생성.
$ vi ~/.zshrc
export HOMEBREW_GITHUB_API_TOKEN=f388c6xxxxxxxxxxxx
# 토큰 확인
$ curl  -i -H "Authorization: token f388c6xxxxxxxxxxxx" https://api.github.com/users/whatever
```

Homebrew 사용법
---------------
### 패키지 설치
```
$ brew install wget
```

### 패키지 비활성화 및 활성화
```
$ brew unlink wget # 일시적으로 비활성화
$ brew link wget   # 활성화
```

### 패키지 목록 업데이트
```
$ brew update      # formula를 업데이트
$ brew upgrade     # 업데이트 패키지를 다시 빌드
```

### 설치된 목록보기
```
$ brew list
```

### 패키지 제거
```
$ brew remove wget
```

### Homebrew 설정 목록
```
$ brew --config
HOMEBREW_VERSION: 2.2.13
ORIGIN: https://github.com/Homebrew/brew
HEAD: 3d9cf83fec45a75af61551f53d25383abe009d31
Last commit: 12 days ago
Core tap ORIGIN: https://github.com/Homebrew/linuxbrew-core
Core tap HEAD: 771578aea930610b3d16d3c238fe057971add9c1
Core tap last commit: 9 days ago
HOMEBREW_PREFIX: /home/linuxbrew/.linuxbrew
HOMEBREW_DISPLAY: :0
HOMEBREW_EDITOR: mvim
HOMEBREW_MAKE_JOBS: 2
CPU: dual-core 64-bit unknown_0x15_0x13
Homebrew Ruby: 2.6.3 => /home/linuxbrew/.linuxbrew/Homebrew/Library/Homebrew/vendor/portable-ruby/2.6.3/bin/ruby
Clang: N/A
Git: 2.17.1 => /usr/bin/git
Curl: 7.58.0 => /usr/bin/curl
Kernel: Linux 5.3.0-46-generic x86_64 GNU/Linux
OS: Linux Mint 19.3 Tricia (tricia)
Host glibc: 2.27
/usr/bin/gcc: 7.5.0
glibc: N/A
gcc: N/A
xorg: N/A
```

### Homebrew를 제거
```
$ cd `brew --prefix`
$ brew prune
$ sudo rm -rf * && cd ~ && sudo rm -rf /home/linuxbrew
$ rm -rf ~/.cache/Homebrew
```
