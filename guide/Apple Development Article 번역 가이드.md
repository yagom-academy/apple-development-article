
# Apple Development Article 번역 가이드

이 페이지에서는 `Apple Development Article` 공유 문서에 대한 참여 방법에 대해서 안내합니다. 지켜야할 규칙이 많아보이지만 어렵지 않답니다 🙂 반드시 숙지 후에 제작에 참여해주시면 감사하겠습니다.

야곰 아카데미의 `Apple Development Article` 공유 문서는 원문과 번역문을 병기하는 방식으로 작성합니다. 아래의 내용을 확인해주세요.

이 페이지는 아래와 같은 순서로 안내합니다.
1. [필독사항](##📮-필독사항)
2. [Apple Development Article 마크다운 작성 요령](##🌳-Apple-Development-Article-마크다운-작성-요령)
3. [작업한 마크다운을 깃북에 추가하는 방법](##🌳-작업한-마크다운을-깃북에-추가하는-방법)
4. [로컬에서 Gitbook으로 결과물을 확인하는 방법](##🌳-로컬에서-Gitbook으로-결과물을-확인하는-방법)

<br>

## 📮 필독사항

- **작업하고자 하는 아티클이 이미 작성되어 있지는 않은지, 이슈에 등록되어있지는 않은지 먼저 확인합니다.**
- **반드시 [`Wiki`](https://github.com/yagom-academy/apple-development-article/wiki/용어-위키) 에서 용어를 참고하여 정확하고 일관된 용어를 사용합니다.**
- `-니다.` 체를 사용합니다. (-하세요. 등 자연스러운 번역을 위한 `-요.` 체는 허용)
- 원문과 번역문을 병기하는 방식으로 작성합니다. 병기 방법은 아래 컨벤션을 따라주세요.
- 번역문에는 원문의 들여쓰기, 링크, 진하기 등 속성을 정확하게 따릅니다.
- 내가 작성(번역)한 문장을 소리내어 읽어보았을 때 숨이 차거나 부자연스럽다면 문장을 짧게 끊어봐도 좋습니다.

<br>

## 🌳 Apple Development Article 마크다운 작성 요령
컨벤션 규칙은 가독성에 많은 영향을 줍니다. 통일감있는 공유 문서 작성을 위해서 마크다운 작성 요령에 대해 정독해주시면 감사하겠습니다. 

다양한 마크다운 편집기가 있지만 결과적으로 최종 마크다운 파일은 아래와 같은 컨벤션을 따라주세요! (마크다운 템플릿과 기존에 작성된 아티클들의 마크다운 파일을 참고해서 보시면 이해하기 더 쉽습니다.)

<img alt="마크다운 템플릿" src="https://user-images.githubusercontent.com/73867548/164189754-7764c3c8-8196-4e20-ba40-ac8ee1adaf8f.png">

### 1️⃣ 타이틀
![무제 001](https://user-images.githubusercontent.com/73867548/162399454-952b3df5-31e0-4eb7-84bf-0eaf9e880c11.jpeg)

### 2️⃣ 줄바꿈 규칙
![무제 002](https://user-images.githubusercontent.com/73867548/164190454-bf4e60fc-5d53-4912-a664-6814ae29873a.jpeg)

### 3️⃣ 번역문 속성
![무제 003](https://user-images.githubusercontent.com/73867548/162399466-5de85ca9-5106-4ab7-a52e-17f61f157c6d.jpeg)

### 4️⃣ 그 외
![무제 004](https://user-images.githubusercontent.com/73867548/162399471-1ed827d0-8ae9-4520-a907-07a358e36ab5.jpeg)

<br>

## 🌳 작업한 마크다운을 깃북에 추가하는 방법
1. 작업한 마크다운 파일의 파일 이름은 `{Article 제목}.md`로 합니다.
    - ex) `Receiving and Handling Events with Combine.md`
2. 작업한 마크다운 파일은 `articles` 폴더에 넣어줍니다.
3. `SUMMARY.md` 파일을 열어 작업한 마크다운을 아래와 같이 추가해줍니다.

    <img alt="image" src="https://user-images.githubusercontent.com/73867548/162395466-4b406c49-c2ec-4e63-8871-f6e9154cd3bf.png">

    - 새로 작업한 문서의 경우 가장 아래에 추가합니다.
    - 마크다운 예시
        > `* [{Article 제목}](articles/{Article 제목}.md)`

<br>

## 🌳 로컬에서 Gitbook으로 결과물을 확인하는 방법

마크다운을 작업하며 Gitbook에서의 결과물이 어떻게 될지 확인하고 싶을 때에는 로컬 서버를 통해서 확인할 수 있습니다. PR을 보내기 전에 로컬 서버로 작업의 결과를 확인해보시고 PR을 보내시기를 권장합니다 :)

Gitbook이 설치되지 않은 경우 아래 명령어를 먼저 실행해주세요.

```
// 1
npm install -g gitbook-cli

// 2
gitbook install
```

<br>

Gitbook이 설치되었다면 작업 후, 터미널에서 Gitbook 폴더로 이동하여 아래 명령어를 순서대로 입력해줍니다.

```
// 1
gitbook build

// 2
gitbook build . docs

// 3
gitbook serve
```

명령어를 순서대로 모두 입력하였다면 로컬 주소(http://localhost:4000)로 이동하여 작업 내역을 확인할 수 있습니다.
