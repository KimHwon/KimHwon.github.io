---
title: macOS에서 jenv 사용하기
date: 2020-02-29
tags:
  - Java
  - macOS
---

# 1. Install jenv

```bash
brew install jenv
```
Install jenv with `brew`


```bash
export PATH="$HOME/.jenv/bin:$PATH"
if which jenv > /dev/null; then eval "$(jenv init -)"; fi
```
Add path to `.bash_profile` or `.zshrc`


```bash
source ~/.bash_profile
source ~/.zshrc
```
Enable `.bash_profile` or `.zshrc`


```bash
jenv enable-plugin export
```
Enable export plugin


# 2. Install JDK
Do whatever you want.


# 3. Register JDK to jenv
```bash
jenv add /Library/Java/JavaVirtualMachines/[JDK version name]/Contents/Home/
```

# 4. Use jenv
  * `jenv versions` : Show JDK version list.
  * `jenv global [JDK version name]` : Enable JDK to default java enviroment.
  * `jenv local [JDK version name]` : Enable JDK to java enviroment in this directory.