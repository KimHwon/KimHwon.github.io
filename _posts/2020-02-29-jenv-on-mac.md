---
title: macOS에서 jenv 사용하기
date: 2020-02-29
tags:
  - Java
  - macOS
keywords:
  - Java
  - Jenv
  - macOS
---

# 1. Install jenv

```bash
brew install jenv
```
`brew`를 사용해서 jenv를 설치한다.


```bash
export PATH="$HOME/.jenv/bin:$PATH"
if which jenv > /dev/null; then eval "$(jenv init -)"; fi
```
jenv의 환경변수를 설정한다.
`.bash_profile` 또는 `.zshrc`에 위 코드를 추가한다.


```bash
source ~/.bash_profile
source ~/.zshrc
```
`.bash_profile` 또는 `.zshrc`를 실행하여 설정을 적용한다.


```bash
jenv enable-plugin export
```
jenv의 Export plugin을 설정한다.
`$JAVA_HOME`등의 가상환경으로 연결을 설정한다.

정상적으로 설정이 완료되었다면, `which java`를 했을 때, `$HOME/.jenv/shims/java`로 경로가 나온다.


# 2. Install JDK
JDK를 설치한다.
`brew`를 사용해도 좋고, Oracle이나 OpenJDK 홈페이지에 접속해서 pkg파일을 다운로드 받은 뒤 설치해도 좋다.


# 3. Register JDK to jenv
```bash
jenv add /Library/Java/JavaVirtualMachines/[JDK version name]/Contents/Home/
```
설치한 JDK를 jenv에 등록한다.

# 4. Use jenv
  * `jenv versions` : 사용할 수 있는 JDK 환경 목록을 보여준다.
  * `jenv global [JDK version name]` : 선택한 JDK 환경을 기본 설정으로 사용한다.
  * `jenv local [JDK version name]` : 현재 디렉토리에서만 선택한 JDK 환경을 사용한다.