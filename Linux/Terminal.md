

## 기본정보 in macOS

* macOS 10.15 부터, default shell 이 `zsh` 이다.
* nvm 은 `.zshrc` 가 update 된 버전을 찾는다.
* 현재 내 terminal 은 `bash` 를 사용하지만, `zsh` 가 깔려있는 상태
* global packages 를 다운로드 받으려 할 때, macOS 에서는 Permission 관련 에러가 뜬다. 그럴땐 `sudo` 명령어를 이용하자.

----

## Node & NPM

#### NPM is a popular command line tool for installing libraries from the Node.js ecosystem.

* [21.04.26 Error] 
  * `command -v nvm` 해도 아무런 반응이 없었음
    * [Troubleshooting on macOS](https://github.com/nvm-sh/nvm#troubleshooting-on-macos) 참고하여 해결
* `nvm install 14.16.0`  : node.js 14.16.0 버전 다운
* `nvm alias default 14.16.0` : 14.16.0 버전을 default로 설정

---

## Flags, arguments

* `npm install -g ionic`
* `npm install --global ionic`  :  -- 붙이면 단어를 넣으면 됨
  * 여기서 `install` 과 `ionic` 이 arguments 에 해당



----

### 출처

* https://ionicframework.com/blog/new-to-the-command-line/
* https://ionicframework.com/docs/intro/environment
* https://github.com/nvm-sh/nvm#uninstalling--removal
* https://github.com/nvm-sh/nvm#install--update-script

   