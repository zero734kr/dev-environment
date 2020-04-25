Linux 개발 환경 구축 가이드
======================

Homebrew 설치
---------------
- cURL은 기본적으로 설치되었다고 가정한다.
    ```
    $ /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
    $ brew doctor # 문제 진단을 해줌.
    $ brew update # 최신버전 업데이트.
    ```

- 참조 문서 : [Homebrew에 설치 및 사용법(상세)][homebrew]

Git 설치
---------------
- 기본 설치
    ```
    $ brew install git
    ```

- 참조 문서 : [Git 설치, 설정, 사용법(상세)][mac_git]

Gnome 터미널 설정
---------------
Gnome 터미널은 최대한 깔끔하게 쓰는게 좋다.
```sh
$ sh -c `curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh` # Oh My ZSH 설치
$ chsh -s /bin/zsh # zsh를 기본 터미널로 설정
$ sudo apt-get install dconf-cli # 필요한 패키지 설치

$ cd ~
$ git clone https://github.com/dracula/gnome-terminal # Dracula Theme 다운로드
$ cd gnome-terminal
$ ./install.sh # Dracula Theme 설치 및 적용

$ cd ~
$ wget https://github.com/tonsky/FiraCode/releases/download/3.1/FiraCode_3.1.zip # FiraCode 폰트 다운로드
$ sudo apt install unzip # zip 파일 압축 해제 패키지 다운로드
$ unzip FiraCode_3.1.zip -d ~/.firacode # ~/.firacode로 압축 파일의 내용물 저장
$ cd .firacode/ttf 
$ sudo mv * /usr/share/fonts # True Type 폰트들을 시스템 폰트 디렉터리로 이동 
$ sudo fc-cache -vf # 폰트 캐시 업데이트
# 그리고 다운로드 받은 Fira Code 폰트를 적재 적소에 사용한다. (예시: vsc)
# 주의: Gnome Terminal에서는 Fira Code 폰트가 작동을 안 한다.

$ git clone https://github.com/denysdovhan/spaceship-prompt.git "$ZSH_CUSTOM/themes/spaceship-prompt"
$ ln -s "$ZSH_CUSTOM/themes/spaceship-prompt/spaceship.zsh-theme" "$ZSH_CUSTOM/themes/spaceship.zsh-theme"
$ nano ~/.zshrc
# ...
ZSH_THEME="spaceship"
# ...

# 저장하려면 CTRL + X -> Y -> ENTER
$ source ~/.zshrc

# 추가 설정
$ sudo apt install xclip
$ nano ~/.zshrc
#...
SPACESHIP_PROMPT_ORDER=( # 프롬프트 정렬 순서
  user          # Username section
  dir           # Current directory section
  host          # Hostname section
  git           # Git section (git_branch + git_status)
  hg            # Mercurial section (hg_branch  + hg_status)
  exec_time     # Execution time
  line_sep      # Line break
  vi_mode       # Vi-mode indicator
  jobs          # Background jobs indicator
  exit_code     # Exit code section
  char          # Prompt character
)
SPACESHIP_USER_SHOW=always
SPACESHIP_PROMPT_ADD_NEWLINE=false
SPACESHIP_CHAR_SYMBOL="❯"
SPACESHIP_CHAR_SUFFIX=" "

alias copy='xclip -sel clip <' # 파일의 내용을 클립보드로 복사한다
#...

# 저장하려면 CTRL + X -> Y -> ENTER
$ source ~/.zshrc

# 플러그인 추가
$ sh -c "$(curl -fsSL https://raw.githubusercontent.com/zdharma/zinit/master/doc/install.sh)"
$ echo "zinit light zdharma/fast-syntax-highlighting\nzinit light zsh-users/zsh-autosuggestions\nzinit light zsh-users/zsh-completions" >> ~/.zshrc
$ source ~/.zshrc
```

위 커맨드들을 실행하면 아래와 같은 화면을 볼 수 있다.

![사진 1][linux_terminal]

커맨드라인 정보는 아래와 같다.
```md
. `zero` : 접속 유저 이름
. `in ~/.firacode` : 현재 디렉터리 경로
. `on  master` : 현재 브랜치
. `on   (git)-[master|merge]-` : merge 브랜치와 master 브랜치가 대치 중
. `[⇕=]` : conflict가 해결되지 않음
. `[⇕✘?]` : conflict 해결을 기다리는 중
. `[⇕+]` : conflict 해결이 된 파일이 스테이지 됨
. `[✘!?]` : 추적되지 않는 파일들과 추적이 되는 파일들이 섞여있음
. `[!]` : `git add`가 실행되지 않음
. `[+]` : `git add`가 실행됨, `git commit`를 기다리는 중
. `[⇡]` : `git commit`가 실행됬으며 `git push`를 기다리는 중
. `took 4s` : 이 작업을 실행하는데 걸린 시간
```
* Fira Code 폰트 사용시에만 보이는 몇몇 아이콘이 있다.

### 2. zshrc 설정 정보
```sh
$ .. : "cd .."와 동일. 이전 데릭토리로 갈 리스트 보여줌.
$ cd - Enter : 과거 커맨드 중복 제거된 히스토리 보여줌
$ cd Enter : 과거에 입력한 커맨드 보여줌.

# alias copy='xclip -sel clip <'
$ `copy ~/.ssh/id_rsa.pub` : "xclip -sel clip < ~/.ssh/id_rsa.pub"와 동일한 명령어로 id_rsa.pub의 내용을 클립보드에 복사.
```

### 3. Vim 설정 정보
```
- 검색 하이라이트 지우기 : ESC 두번 클릭
- 중괄호({}), <>, () 대응.
- 문자 코드 자동 인식
- 개행 코드 자동 인식
- (중)괄호 자동 완성
- 폴딩
	. zc : 현재 커서가 위치한 곳의 가장 안쪽의 fold 접기.
	. zm : 전체적으로 제일 안쪽에 위치한 모든 fold 접기.
	. zo : 현재 커서가 위치한 곳의 가장 바깥쪽의 fold 펼치기.
	. zr : 전체적으로 제일 바깥쪽에 위치한 모든 fold 펼치기.
```

- 단축키 정보
![사진 2][vim_image]

-  참조 문서 : [vim 점진적으로 학습하기][vim]

============

[homebrew]: https://github.com/zero734kr/dev-environment/blob/master/linux-homebrew.md
[linux_git]: https://github.com/zero734kr/dev-environment/blob/master/linux-git.md
[linux_terminal]: https://i.imgur.com/h3jCOVj.png#.XqScOeVdwno.link
[vim]: http://www.mimul.com/pebble/default/2014/07/15/1405420918073.html
[vim_image]: http://www.mimul.com/pebble/default/images/blog/tech/vi-vim-cheat-sheet-ko.png