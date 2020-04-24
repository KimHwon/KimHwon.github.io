---
title: Ubuntu 18.04 에서 카카오톡 설치
date: 2020-04-23
tags:
  - ReferenceDocument
  - Ubuntu
  - Wine
keywords:
  - Ubuntu
  - Wine
  - Kakotalk
  - 우분투
  - 카카오톡
---

한국에서 가장 많이 사용하는 인스턴트 메신저인 카카오톡은 안타깝게도 Ubuntu를 지원하지 않습니다. 따라서 Wine을 사용하여 Windows용 카카오톡 PC를 설치할 수 밖에 없습니다.

나중에 참고할 수 있도록 간단하게 기록을 남깁니다.

# Wine 설치

## 1. 32bit Architecture 설정
```bash
sudo dpkg --add-architecture i386
```

## 2. Apt repository key 다운로드 및 등록
```bash
wget -nc https://dl.winehq.org/wine-builds/winehq.key
sudo apt-key add winehq.key
```

## 3. Wine repository 등록
※ Wine 4.5 이상부터는 libfaudio0 패키지가 필요한데, Ubuntu 19.10 이하에서는 제공되지 않습니다. 따라서 `cybermax-dexter/sdl2-backport` PPA를 추가해야합니다. 등록한 PPA들은 **소프트웨어 & 업데이트** 앱에서 기타 소프트웨어 탭에서 관리할 수 있습니다.
```bash
sudo apt-add-repository 'deb https://dl.winehq.org/wine-builds/ubuntu/ bionic main'
sudo add-apt-repository ppa:cybermax-dexter/sdl2-backport
```

## 4. Wine 설치
```bash
sudo apt update
sudo apt install --install-recommends winehq-stable
```

## 5. Wine 환경 설정
혹시 모를 호환성 이슈를 막기 위해 32bit 프로그램만 실행하도록 설정하고 설정파일의 경로를 지정합니다.
```bash
mkdir ~/.wine
WINEARCH=win32 WINEPREFIX=~/.wine LANG="ko_KR.UTF-8" winecfg
```

**Wine 설정** 윈도우가 열립니다. 상황에 맞게 적절한 옵션을 설정합니다. <br>
여기서는 화면 해상도를 192dpi로 설정했습니다.

## 6. 한글 폰트 설치
미리 다운로드 받아둔 `gulim.ttf`를 wine 내부로 복사하고 레지스트리를 편집합니다.
```bash
cp gulim.ttf ~/.wine/drive_c/windows/Fonts
sed -i 's/"MS Shell Dlg".*/"MS Shell Dlg"="Gulim"/g' ~/.wine/system.reg
sed -i 's/"MS Shell Dlg 2".*/"MS Shell Dlg 2"="Gulim"/g' ~/.wine/system.reg
```

# 카카오톡 설치

## 1. KakaoTalk_Setup.exe 다운로드
https://software.naver.com/software/summary.nhn?softwareId=GWS_000083

## 2. KakaoTalk_Setup.exe 설치
```bash
wine KakaoTalk_Setup.exe
```
또는 파일탐색기를 열고 exe에 우클릭하여 **Wine Windows Program Loader로 열기**를 선택할 수 있습니다.

이어서 설치를 진행합니다.

## 3. 시스템 트레이 설정
```bash
sudo apt install gnome-shell-extension-top-icons-plus
```
설치가 끝난 후, **기능 개선** 앱에서 확장 탭을 열고 **Topicons plus**를 활성화합니다.

---

## 참고
* https://wiki.winehq.org/Wine_User%27s_Guide
* http://ubuntuhandbook.org/index.php/2020/01/install-wine-5-0-stable-ubuntu-18-04-19-10/
* https://hiseon.me/linux/ubuntu/ubuntu-kakaotalk/