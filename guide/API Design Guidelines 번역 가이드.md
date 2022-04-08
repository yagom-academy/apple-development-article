
# Apple Development Article 번역 가이드

이 페이지에서는 `Apple Development Article` 공유 문서에 대한 참여 방법에 대해서 안내합니다. 지켜야할 규칙이 많아보이지만 어렵지 않답니다 🙂 반드시 숙지 후에 제작에 참여해주시면 감사하겠습니다.

야곰 아카데미의 `Apple Development Article` 공유 문서는 원문과 번역문을 병기하는 방식으로 작성합니다. 아래의 내용을 확인해주세요.

이 페이지는 아래와 같은 순서로 안내합니다.
1. 필독사항
2. Apple Development Article 마크다운 작성 요령
3. 마크다운을 깃북에 추가하는 방법
4. 로컬에서 깃북으로 결과물을 확인하는 방법

<br>

## 📮 필독사항

- **반드시 [`Wiki`](https://github.com/yagom-academy/swift-doc-kor/wiki/용어-위키) 에서 용어를 참고하여 정확하고 일관된 용어를 사용합니다.**
- `-니다.` 체를 사용합니다. (-하세요. 등 자연스러운 번역을 위한 `-요.` 체는 허용)
- 원문과 번역문을 병기하는 방식으로 작성합니다. 병기 방법은 아래 컨벤션을 따라주세요.
- 번역문에는 원문의 들여쓰기, 링크, 진하기 등 속성을 정확하게 따릅니다.
- 내가 작성(번역)한 문장을 소리내어 읽어보았을 때 숨이 차거나 부자연스럽다면 문장을 짧게 끊어봐도 좋습니다.

<br>

## ✍🏻 원문과 번역문 병기하여 작업하기

원문과 번역문은 아래와 같은 형식으로 병기합니다.

### 🔎 공유 문서 최종

<img alt="image" src="https://user-images.githubusercontent.com/73867548/157362639-ce82a9b3-e618-4bff-bd2d-27852ac9eac0.png">



### 🔎 마크다운

<img alt="image" src="https://user-images.githubusercontent.com/73867548/156737933-cb84c4de-e524-4f49-8082-52f73dd8ff3d.png">

- 번역문은 반드시 **원문 아래에** 작성합니다.
- 원문은 회색, 이탤릭체로, 번역문은 속성을 추가하지 않고 병기합니다.
- 번역문의 양식은 원문의 양식과 **완전 일치하도록** 작성합니다. 아래의 작업 팁을 참고해주세요.

<br>

## ✍🏻 작업 팁

1. 저장소를 clone합니다.
2. `APIDesignGuidelines - 담당 파트의 마크다운` 으로 접근합니다.
3. 들여쓰기와 줄바꿈 등 번역문의 양식은 원문의 양식과 완전히 일치하도록 작성합니다. 
4. 코드 블록의 주석으로 처리된 영문은 바로 아래에 주석을 추가하여 한글로 번역합니다.
    
    ```markdown
    // This is an example sentence.
    // 예시 문장입니다.
    
    some code..
    ```
    
5. 제목은 괄호를 활용하여 번역합니다. (ex: `Fundamentals (핵심 개념)`)
6. 번역의 단위는 마크다운에서 색상, 이탤릭체 등의 html이 적용된 단위로 번역합니다. 아래 예시를 확인해주세요.
    
    > `**<i><span style="color: #C0C0C0">**`**Clarity at the point of use** is your most important goal. Entities such as methods and properties are declared only once but used repeatedly. Design APIs to make those uses clear and concise. When evaluating a design, reading a declaration is seldom sufficient; always examine a use case to make sure it looks clear in context.`**</span></i>**`
    
    > API 설계의 가장 중요한 목적은 **사용되는 시점에서의 명료함**입니다. 메서드나 프로퍼티와 같은 개체는 한 번만 선언되지만 반복적으로 사용됩니다. API는 이러한 개체들을 명확하고 간결하게 사용할 수 있도록 설계해야합니다. API 설계가 제대로 되었는지 평가할 때에 코드의 선언부만 읽는 것으로는 충분하지 않습니다. 항상 문맥상에서 명확하게 사용되는지를 살펴야합니다.
