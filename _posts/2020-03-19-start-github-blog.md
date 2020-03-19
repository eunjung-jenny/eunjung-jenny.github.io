---
title: "깃허브 블로그 시작하기"
date: 2020-03-19 15:17
tags: [github, blog, jekyll]
---

~~지킬테마를 사용하면 쉽고 빠르게 만들 수 있다고 하던데~~ 서너 시간 걸려서 깃허브 블로그를 드디어 띄웠다.<br/><br/>모두 같은 마음이겠지 하는 생각으로 어떻게 했는지를 정리. 본문에서 언급하는 내용과 관련된 레퍼런스들은 제일 아래에 모아두었다.<br/><br/> 깃허브 블로그가 어떻게 동작하는거지? 라는 의문을 갖고 있고 이를 해결하고자 하는 분들께는 이 글이 전혀 도움이 되지 않을 거고, 다 모르겠고 그냥 쉽게쉽게 깃허브에서 블로그 하고 싶다 하는 분들께 조금이나마 도움이 되길..

## 0. 조건

- 맥 OS

- 지킬 테마, 그 중에서도 **minimal mistakes**를 사용하였다. 다른 테마를 사용할 경우 아래 내용은 일부 적용되지 않는 부분이 있을 수 있으니 그럴 때는 해당 테마에 대한 가이드를 찾아보길.
- 아래와 같이 하면 블로그 URL 은 `[유저명].github.io` 가 된다.

## 1. 기본 세팅으로 블로그 띄우기만 하기

### 깃허브에 `[유저명].github.io` 로 레포 생성

- 내 경우, eunjung-jenny 가 유저명이므로, `eunjung-jenny.github.io` 로 레포 생성했다.

### 내 컴퓨터에 깃허브 레포를 클론한다.

```bash
git clone [레포url] [생성될 폴더명]
# 생성될 폴더명은 별도로 작성하지 않을 경우 레포명과 동일한 폴더가 생성됨
```

### 생성된 폴더 내에서 아래와 같이 작업한다.

- `Gemfile` 이란 이름으로 파일을 생성하고 아래와 같이 작성

  ```ruby
  source "https://rubygems.org"

  gem "github-pages", group: :jekyll_plugins
  ```

* `_config.yml` 파일을 생성한 뒤, [minimal mistakes 레포](https://github.com/mmistakes/minimal-mistakes) 의 `_config.yml` 파일 본문을 일단 복붙 후 본문 중 해당되는 부분을 다음과 같이 수정

  ```yaml
  remote_theme: mmistakes/minimal-mistakes # 더블쿼터 없음! 더블쿼터로 감싸면 github 에서 warning 메일을 받게 됩니다..
  url: "https://[유저명].github.io"
  baseurl: "" # 빈 문자열
  ```

  - minimal mistakes 사용 가이드에는 `plugins` 항목에 `jekyll-include-cache` 를 추가하라고 하는데 실제로는 이미 추가되어 있었다.

- `index.html` 파일 생성한 뒤, [minimal mistakes 레포](https://github.com/mmistakes/minimal-mistakes) 의 `index.html` 파일 본문을 복붙

- `bundle` 이라는 명령어 실행~~해야 하는데...~~

  - 루비 의 패키지 매니저인 `gem` (Node.js 의 npm, Python 의 pip 격인듯?) 을 통해 `bundle` 을 설치하면 된다고..

    ```bash
    gem install bundler
    ```

  - 했는데 _Error: 어쩌고 저쩌고.. You don't have write permissions for 어쩌고 저쩌고_... `bundle`을 설치할 권한이 없다고 한다... [구글링으로 나를 도와주실 분을 찾았다.](https://jojoldu.tistory.com/288)

    ```bash
    # 맥에서만 가능!
    brew update
    brew install rbenv ruby-build # rbenv 설치 (루비 env 인듯)
    rbenv versions # 제대로 설치됐나 확인
    # 위 명령어의 결과로 *system (어쩌고저쩌고) 가 나오면, system 이 아닌 rbenv 로 관리되는 ruby 를 설치해야 한다고 함

    rbenv install -l # 설치할 수 있는 ruby 버전 확인
    # 나타나는 항목들 중 숫자로만 구성된 제일 마지막 버전을 설치할 것임 (20.03.19 기준 2.7.0)
    rbenv install 2.7.0
    rbenv versions # 제대로 설치됐나 확인
    # 위 명령어의 결과로 system 아래에 2.7.0 이 보일 것

    rbenv global 2.7.0
    rbenv versons # 제대로 변경됐나 확인
    # 위 명령어의 결과로 system 이 아닌 2.7.0 앞으로 * 표시가 옮겨졌을 것

    # 마지막으로... 환경변수 설정을 위해 vim...
    vim ~/.zshrc # bash 를 사용하는 사람은 .zshrc 대신 .bash 를 입력하면 될 듯

    # 이상한 화면이 보일텐데 잘 모르겠고 시키는대로 제일 아래에 아래와 같이 추가해준다.
    ## 잘 모르겠으니 공백까지 똑같이 작성하고, 저장하고, vim 을 빠져나온다.
    ### 입력할 때는 a, A, i, I, o, O 중 아무거나 누르고 제일 아래로 이동
    [[ -d ~/.rbenv ]] && \
      export PATH=${HOME}/.rbenv/bin:${PATH} && \
      eval "$(rbenv init -)"
    ### 저장할 때는 :w + 엔터, 나갈 때는 :q + 엔터

    source ~/.zshrc # 수정한 내용을 적용

    # 대망의
    gem install bundler

    bundle # 이걸 하고 싶었던 것..!
    ```

### .gitignore 파일 생성 후 `Gemfile.lock` 추가

- 이 단계까지 오면 언제 생겼는지 모를 `Gemfile.lock` 파일이 생성되어 있을텐데, 잘 모르겠지만 `minimal mistakes` 레포에도 이 파일이 올라와있지 않고, 파일명의 느낌상 보안과 관련될 것 같으므로 `.gitignore` 파일을 생성하여 본문에 `Gemfile.lock` 을 추가해주었다.

### 지금까지의 내용을 add, commit, push!

```bash
git add .
git commit -m "[커밋제목]"
git push origin master
```

- `[유저명].github.io` 로 접속해본다! _깃허브 페이지가 업데이트 사항이 바로 적용되지는 않는 것 같으니 몇 분 정도 기다렸다가 시도해볼 것!_

## 2. 내 블로그라고 표시하기

- 이 부분은 사실 minimal mistakes 에서 제공하는 가이드를 보는 게 제일 확실하고 자세한데,, 여기서는 내가 처음에 수정했던 제일 기본적인 것들만.... 필수적인 게 아니고 전부 개인의 선택. ([이 부분은 가이드의 Customization - configuration](https://mmistakes.github.io/minimal-mistakes/docs/configuration/))

```yaml
# theme 이라고 구분된 항목
minimal_mistakes_skin: "dark" # skin 모양을 결정하는데, 가이드에서 어떤 옵션이 있는지 구경 가능

# Site Settings 항목
locale: "ko-KR" # 언어 : 다른 언어를 쓰려면 가이드 참고
title: "[블로그 이름]"
name: "[블로그 주인 이름]"
repository: "[유저명/레포명]"

# Site Author 항목
author:
  name: "[블로그 주인 이름]"
  # avatar: 설정하지 않았으나 이미지 경로 넣어주면 블로그에 표시됨
  bio: "[블로그 주인 소개]"
  location: "[위치]"
  email: "[이메일 주소]" # 블로그 주인한테 바로 메일 보낼 수 있게 해줌
  # link 블로그에 달아놓고 싶은 링크가 있으면 작성
```

- 잊지 말고 add, commit, push!

## 3. 포스팅하기

### 폴더 내에 `_post` 폴더 생성

- 기본적으로 `_post` 내의 마크다운 파일을 하나의 글로 인식하고 배포해준다.

### 마크다운 작성 규칙

- 제목: `YYYY-MM-DD-[파일명].md`
- 본문

```markdown
---
title: "깃허브 블로그 시작하기"
date: 2020-03-19 13:40
tags: [github, blog, jekyll]
---

본문 내용 작성
```

- 본문의 title, date, tags 등의 정보에 대해 자동으로 포맷 변환이 이뤄진다.

## 4. 마무리

힘들다. 블로그를 잘 하는 편도 아닌데 한 번 해볼까 했다가 고생했다.<br/> 이 글이 마지막 글이 될지도 모르겠다만 깃허브에서 블로그 하고 싶은데 어떻게 동작하는건지 그런거 자세히 알고 싶지 않다 하는 분들께 도움이 되면 좋겠다.

## Reference

- (jekyll theme -minimal mistakes- repo) https://github.com/mmistakes/minimal-mistakes
- (jekyll theme -minimal mistakes- guide) https://mmistakes.github.io/minimal-mistakes/docs/quick-start-guide/
- (깃허브 블로그 시작 튜토리얼) https://dreamgonfly.github.io/2018/01/27/jekyll-remote-theme.html
- (깃허브 블로그 시작 튜토리얼) https://zoomkoding.github.io/gitblog/2019/08/15/git-blog-1.html
- (mac & gem 권한 부여) https://jojoldu.tistory.com/288
- (vim 입력) https://opentutorials.org/course/730/4562
- (vim 저장) https://opentutorials.org/course/730/4561
- (github posting) https://devyurim.github.io/development%20environment/github%20blog/2018/01/01/blog-2.html
- (github warning) https://github.com/mmistakes/minimal-mistakes/issues/1394
- (git commit, push cancel) https://gmlwjd9405.github.io/2018/05/25/git-add-cancle.html
