# Mobile accessibility


## 참고 자료
[(KS X 3253)모바일 애플리케이션 콘텐츠 접근성  지침 2.0.pdf](https://prod-files-secure.s3.us-west-2.amazonaws.com/e2f62e73-f32c-4af7-a7d2-7705e9f9082c/4b83a932-d51d-4681-92a1-9363db8faa8d/(KS_X_3253)%EB%AA%A8%EB%B0%94%EC%9D%BC_%EC%95%A0%ED%94%8C%EB%A6%AC%EC%BC%80%EC%9D%B4%EC%85%98_%EC%BD%98%ED%85%90%EC%B8%A0_%EC%A0%91%EA%B7%BC%EC%84%B1__%EC%A7%80%EC%B9%A8_2.0.pdf)

[‘W3C 웹 접근성 지침’](https://www.w3.org/WAI/standards-guidelines/ko)

### 접근성 검사기 종류
    - 특징
        - 대략적 문제점과 종류 파악
        - 웹 접근성 양적 측정
        - 신속성, 편리성, 저비용, 보편화 등등 장점
        - 한국형 웹 콘텐츠 접근성 지침 2.0 기준
        - 전문 검사와 차이점이 매우 많아, 못 찾아내는 것이 많음.
    - [K-WAH 4.0](https://www.wa.or.kr/board/view.asp?sn=10025&page=1&search=&SearchString=&BoardID=0004&cate=)
        - 기존 프로그램들과 다르게 웹 페이지들을 하나씩 평가하지 않고 웹 페이지들 전체를 통합하여 평가
        - 각 웹 페이지들을 구체적으로 살펴볼 수 없어 어느 웹 페이지의 특정 기능의 문제점은 파악하기 어려움
        
        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/e2f62e73-f32c-4af7-a7d2-7705e9f9082c/86ffdd42-77ea-43c7-a868-880533997dfc/image.png)
        
    - A-Prompt 평가도구
    - Boddy
    - LIFE
    
    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/e2f62e73-f32c-4af7-a7d2-7705e9f9082c/67da7f92-d4f3-47b9-b4ba-d1ba3bac9c08/image.png)
    
### 전문 검사
    
    - 특징
        - 웹접근성 정도, 상세 내용 분석 파악 용이
        - 웹접근성연구소 참고
    - [오픈왁스 사용법](https://blog.naver.com/chae-yul/221187023938)
        - 평가 항목을 일일이 육안으로 확인하여 질적 측면 측정
        - 크롬에 표기되지 않는 항목들이 존재함 ⇒ 인터넷 익스플로어 사용하여 구글 크롬에서 평가하는 동일한 웹 페이지를 대조하며 평가 필요.
    - **명도 측정기 : Colour Contrast Analyser 2.2**
        - 주 메뉴를 선택할 시 나타나는 모든 서브메뉴의 명도 대비 지침 기준 : **4.5: 1**
        - **명도 대비 중요!**
        - 배경과 콘텐츠들, 텍스트들, **배경 간에도**
    - [웹표준 문법검사 W3C Validator](https://validator.w3.org/)
    - 화면 낭독기, HPR 스크린리더
        - 시각장애인 위주
        - TAP 키로 넘어가는지
- 접근성 오류율 : 5% 목표

## 공통

- [기본 언어 표시](https://honeystorage.tistory.com/325)
- `title` 속성은 모바일에서 사용자에게 전달되지 않을 수 있음.
- [콘텐츠의 선형화](https://nuli.navercorp.com/guideline/s03/g19)
- 닫는 태그인 *슬래스(/)* 반드시 사용
- 입력 서식 시 권장사항
    - 입력 서식에서는 용도와 목적을 알 수 있는 대체 정보를 제공
    - 특정 서식이 잘못 되었다면, 오류 내용을 알려줄 수 있어야 함.
    - 오류 시에 초점 자동 이동
- 폰트 상대 단위 : **`em, rem`**

## [1. 대체텍스트 제공](https://brunch.co.kr/@tigrisdesign/18)

- 비텍스트 콘텐츠에 `alt="50% 캐릭터”` 태그 사용
- 특정 태그 : `aria-label=”뒤로가기”` ?????
- [IR 기법 사용](https://nuli.navercorp.com/guideline/s01/g01)
- 반드시 alt 는 있어야 함. 의미가 없으면 비워놓는 형태
- 버튼, 이미지, 레이블은 스크린리더에서 태그만 적절히 사용 시 읽어주기에 중복으로 텍스트 내용 포함 X
    
    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/e2f62e73-f32c-4af7-a7d2-7705e9f9082c/57cecb33-c92d-412a-b405-6d9e1e412868/image.png)
    
- 자세한 설명 제공
    - 내용의 명확성, 짧게 핵심만 **(60자 이내)**
    - **자세한 설명이 필요한 경우 설명 텍스트 권장 :**  `longdesc=""` 속성과 설명 링크를 사용할 것을 권장
    - **설명텍스트(화면에 구현되는 텍스트) : 블릿 이미지, 스페이스홀더, 버튼 이미지 등에서는 제외**
    - [의미가 없는 블릿](https://blog.naver.com/patanus/221717779290)과 Spaceholder에서 오류 발생 시, 육안 확인 필요
        - 블릿 이미지: 장식적 기호 또는 디자인으로 사용된 이미지
        - 스페이스홀더 : 여백 및 공백으로 사용된 이미
        - 자동 평가 도구에서 `longdesc=""` 태그 유무로 판단
        - **블릿 이미지 권장 사항: `alt="”`**
        - **스페이스홀더, 버튼 이미지 권장 사항 : `alt=”관련 내용”`**

## 2. 깜빡거림 (Flicker)

- 의심되는 개체에 대해 육안을 통한 확인 필요 ⇒ 일반 이미지도 자동 검사 시 오류 발생 문제점

### [Font Flicker](https://juo1221.github.io/TIL/10/)

### [Background Image Flicker](https://blog.naver.com/jjjhyeok/20043305098)

## 3. Navigation

### [하이퍼링크](https://coding-lks.tistory.com/166)

- 모두 오류로 판정 ⇒ 아래의 해결책으로 해결

### [링크 용도나 목적지의 명확성](https://m.blog.naver.com/polky0421/221480405254)

- 링크텍스트의 명확성을 위해 재변환
- 링크 텍스트 한도 : 60자 미만 권장 - 상황 별로 판단

### 반복 콘텐츠

- 반복되는 네비게이션은 스크린 리더 사용자가 건너뛸 수 있는 `‘Skip to content`’ 링크

### 중복된 링크 사용 비권장

- 두 개 이상의 링크가 한 페이지에서 보여줄 수 있는 내용 ⇒ 의미 파악으로 판단

### [이미지맵](https://blog.naver.com/artbank/222206044864)

- 대체텍스트 있는 것을 권장 - *전국지도 예시*
- 링크 유무에 관계없이 모든 하위 조직을 `<area>`로 마크업하고, 각 항목에 대한 관계를 포함한 `alt` 속성을 명확하게 제공
- 각 `<area>` 에 대한 `alt=""`텍스트는 실제 이미지 설명보다는 링크의 목적지를 가르키는 것 권장
- 각 활성 영역은 마우스 말고도 키보드로도 접근 가능하게 권장
- http://www.haeppa.kr/14

### 팝업창

- (참고)
- 모달 창, 드롭다운 메뉴, 팝업 등의 요소가 활성화되면 해당 요소로 포커스가 이동하고, 닫힐 때 원래 위치로 포커스가 돌아가도록 합니다.

## 4. 초점

### : 모든 객체에는 초점이 적용되고, 초점은 순차적으로 이동

- 선택된 객체는 초점이 적용되었다고 하고 , 초점은 화면상에서 **테두리나 하이라이트로 표시**하는 것이 바람직

- **구현 방법** : 안드로이드에서 제공하는 **기본 사용자 인턴페이스 컴포넌트**(Native UI Component)에서 IOS와 달리 대부분의 기본값이 비활성화 되어 제공된다. 초점 이동이 필요한 UI 경우에는 반드시 `focusable` 속성을 활성화 `= true`시켜야 함
    
    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/e2f62e73-f32c-4af7-a7d2-7705e9f9082c/d886f2e9-99c2-43dd-a9c3-36a3e6e4722a/image.png)
    
    버튼, 입력 필드, 드롭다운 등 각 요소의 역할을 명확하게 지정해야 하며, 복잡한 UI 구성 요소는 `ARIA 속성(예: aria-label, aria-labelledby, aria-describedby)`을 사용해 스크린 리더가 콘텐츠를 적절히 설명
    

## 5. 알림 기능

- 만일 알림을 제공할 때 한가지 방법으로 알람을 울리지말고 , 팝업과 진동을 동시에 울리는 등 .
진동과 시각 , 소리 등 최대한 다양한 방법으로 사용자가 조절하고 선택할 수 있도록 해야한다. (시각 장애인 고려)
- 구현 방법: **Native UI 알림 기능 사용하기**

## 6. 페이지 논리적 구성

- 의미에 맞는 적절한 HTML 태그를 사용 **(시맨틱 마크업)**
- 시맨틱 HTML 요소(<header>, <nav>, <main>, <section>, <footer>, <article>, <button>, <form> 등)

## 7. 온라인 서식 구성

- **키보드만으로도 온라인 서식 입력 및 서식 간 이동이 가능**
- 키보드 사용 **(다른 브라우조나 다양한 보조기구 사용 시 일관된 내용 초점)**
    - 초점 이동 : 탭으로 이동하는 데 안됨.
    - 키보드 사용 보장
    - 자주 사용하는 기능 순서 적절히 배치
    - Tap 클릭의 결과가 논리적이여야 한다.
    - **Enter, space로 클릭이나 선택 기능 제공**
- **세부항목의 접근시 마우스 클릭은 3회 이하**
- 오류 정정에 대한 안내페이지 **(에러 시 도움말)**
    - **입력 폼에 오류가 발생하면 그 오류를 명확하게 알려줘야 하며, 오류가 발생한 필드가 무엇인지, 해결 방법은 무엇인지 쉽게 이해할 수 있어야 합니다**. `ARIA의 aria-live` 속성 등을 활용하면 동적으로 발생하는 오류 메시지를 화면 리더가 인식할 수 있게 할 수 있습니다.
- 콘텐츠 구현 시 전반적인 접근성 준수를 위해 **레이아웃 구성은 css 사용 권장**, 그 외 **팝업창 사용을 지양**하고 **포커스는 팝업창으로 이동시키지 말아야 함.**

## 8. 텍스트 구성

- 텍스트 작성 시 **구두점, 특수기호의 사용을 배제**

- 주의점
    - 가장 잘 지켜진 웹 접근성 요건 10위
    
    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/e2f62e73-f32c-4af7-a7d2-7705e9f9082c/e7ef6511-3016-432a-9b29-7eb194d674ad/image.png)
    
    - 가장 잘 지켜지지 않은 웹 접근성 요건 10위
    
    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/e2f62e73-f32c-4af7-a7d2-7705e9f9082c/7f6664ce-9e31-4b4f-8071-8eb351891a1c/image.png)
