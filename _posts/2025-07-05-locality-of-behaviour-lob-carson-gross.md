---
layout: post
title: 행동의 지역성 (Locality of Behaviour - LoB) - Carson Gross - 번역글
categories: misc
author:
  - 심재훈
---
\[원문: </> htmx ~ Locality of Behaviour (LoB)\]([https://htmx.org/essays/locality-of-behaviour/](https://htmx.org/essays/locality-of-behaviour/))

Carson Gross

2020년 5월 29일

\> “쉬운 유지보수를 위한 주요 특징은 지역성(locality)입니다. 지역성이란 프로그래머가 소스 코드의 작은 부분만 보고도 해당 소스를 이해할 수 있게 하는 특성입니다.” – \[Richard Gabriel\]([https://www.dreamsongs.com/Files/PatternsOfSoftware.pdf](https://www.dreamsongs.com/Files/PatternsOfSoftware.pdf))

<br>

\## <span style="box-sizing: inherit; font-family: 'Microsoft Yahei', '맑은 고딕', 'Malgun Gothic', sans-serif !important; font-weight: 400; overflow-wrap: break-word; word-break: keep-all; background-color: transparent;">LoB 원칙</span>

<span style="box-sizing: inherit; font-family: 'Microsoft Yahei', '맑은 고딕', 'Malgun Gothic', sans-serif !important; font-weight: 400; overflow-wrap: break-word; word-break: keep-all; background-color: transparent;">행동의 지역성(Locality of Behaviour)은 다음을 의미하는 원칙입니다:</span>

\> 코드 단위의 행동은 해당 코드 단위만 보더라도 가능한 한 명확해야 한다.

<br>

\## 논의

LoB 원칙은 \[Richard Gabriel\]([https://www.dreamsongs.com/)의](https://www.dreamsongs.com/\)의) 인용문을 간단하고 규범적으로 공식화한 것입니다. 가능한 한, 그리고 다른 고려사항들과 균형을 이루면서, 개발자들은 "코드 요소의 행동"이 코드확인시 명확하도록 노력해야 합니다.

(원문: developers should strive to make the behaviour of a code element obvious on inspection)

HTML에서 \\\*AJAX 요청을 구현하는 두 가지 다른 방식을 고려해 봅시다. 첫 번째는 \[htmx\]([https://htmx.org/)입니다](https://htmx.org/\)입니다):

AJAX: Asynchronous JavaScript and XML (비동기 스크립트로 페이지 일부만을 전체 페이지 새로고침없이 바꿀 수 있는 기술, AJAX -> jQuery -> React, htmx로 변화해옴)

\`\`\`html

<button hx-get="/clicked">Click Me</button>

\`\`\`

<span style="color: rgb(12, 12, 12); font-family: 'Microsoft Yahei', '맑은 고딕', 'Malgun Gothic', sans-serif; font-size: 16px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: left; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; background-color: rgb(255, 255, 255); text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial; display: inline !important; float: none;">두 번째는</span>\[ jQuery\]([https://jquery.com/)<span](https://jquery.com/\)<span) style="color: rgb(12, 12, 12); font-family: 'Microsoft Yahei', '맑은 고딕', 'Malgun Gothic', sans-serif; font-size: 16px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: left; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; background-color: rgb(255, 255, 255); text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial; display: inline !important; float: none;">입니다:</span>

\`\`\`javascript

$("#d1").on("click", function(){

$.ajax({

/\* AJAX options... \*/

});

});

\`\`\`

\`\`\`html

<button id="d1">Click Me</button>

\`\`\`

<br>

전자의 경우, 버튼 요소의 행동은 검사 시 명확하여 LoB 원칙을 만족합니다.

후자의 경우, 버튼 요소의 행동이 여러 파일에 분산되어 있습니다. 코드베이스에 대한 전체적인 지식 없이는 버튼이 정확히 무엇을 하는지 알기 어렵습니다. 이러한 "원거리 유령 현상(spooky action at a distance)"은 유지보수 문제의 원인이 되며 개발자가 코드베이스를 이해하는 데 방해가 됩니다.

htmx 예시는 좋은 행동의 지역성을 보여주는 반면, jQuery 예시는 행동의 지역성이 좋지 않습니다.

<br>

\### 행동 표면화 vs. 구현 인라인화

행동의 지역성에 대한 일반적인 반론은 코드 단위 내에 구현 세부 사항을 인라인화하여 코드 단위를 덜 추상적이고 더 취약하게 만든다는 것입니다. 그러나 어떤 행동의 구현을 인라인화하는 것과 어떤 행동의 호출(또는 선언)을 인라인화하는 것 사이의 구분을 짓는 것이 중요합니다.

대부분의 프로그래밍 언어에서 함수를 생각해 보세요. 함수의 선언과 호출 지점에서의 사용 사이에는 구분이 있습니다. 좋은 함수는 구현 세부 사항을 추상화하지만, 원거리 유령 현상 없이 명확한 방식으로 호출됩니다.

다른 조건이 동일하다면, "요소의 행동 명확성"을 높이는 것은 좋은 일입니다. 하지만 LoB를 가능한 한 쉽고 개념적으로 깔끔하게 만드는 것은 최종 개발자, 특히 프레임워크 개발자의 몫입니다.

<br>

\### 다른 개발 원칙과의 충돌

LoB는 종종 다른 소프트웨어 개발 원칙들과 충돌할 수 있습니다. 두 가지 중요한 원칙은 다음과 같습니다:

\* **\[DRY - 반복하지 마라 (Don’t Repeat Yourself)\](**[**https://en.wikipedia.org/wiki/Don%27t\_repeat\_yourself)**](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself\)**)

소프트웨어 개발자들은 일반적으로 코드나 데이터의 중복을 피하려고 노력합니다. 이를 "DRY 유지하기", 즉 "반복하지 마라"라고 부릅니다. 다른 소프트웨어 설계 원칙과 마찬가지로, 이 원칙 자체는 좋은 것입니다. 예를 들어, htmx는 DOM의 부모 요소에 많은 속성을 배치하여 자식 요소에 이러한 속성을 반복하는 것을 피할 수 있게 합니다. 이는 DRY를 위해 LoB를 위반하는 것이며, 이러한 절충은 개발자가 신중하게 이루어져야 합니다.

행동이 영향을 미치는 코드 단위에서 멀어질수록 LoB 위반의 심각성은 더 커진다는 점에 유의하십시오. 코드 단위에서 몇 줄 이내에 있다면 페이지 하나만큼 떨어져 있는 것보다 덜 심각하고, 완전히 별도의 파일에 있는 것보다 덜 심각합니다.

확고한 규칙은 없으며, 소프트웨어 개발자로서 주관적인 절충이 이루어져야 합니다.

\* **SoC - 관심사 분리 (Separation Of Concerns)**

관심사 분리는 컴퓨터 프로그램을 별개의 섹션으로 분리하여 각 섹션이 별개의 관심사를 다루도록 하는 설계 원칙입니다. 이에 대한 전형적인 예시는 HTML, CSS, JavaScript를 분리하는 것입니다. 다시 말하지만, 이 원칙 자체만으로는 좋은 것일 수 있습니다. 최근에는 \[인라인 스타일이 더 보편화되었지만\]([https://tailwindcss.com/](https://tailwindcss.com/)), 이와 관련하여 SoC를 지지하는 강력한 주장들도 여전히 존재합니다.

그러나 SoC는 LoB와 충돌한다는 점에 유의하십시오. CSS 파일을 수정함으로써 요소의 모양과 어느 정도 행동이 극적으로 변할 수 있으며, 이러한 극적인 변화가 어디서 왔는지 명확하지 않습니다. 도구가 어느 정도 도움이 될 수 있지만, 여전히 "원거리 유령 현상"이 발생하고 있습니다.

다시 말하지만, 이것은 SoC를 전면적으로 비난하는 것이 아니라, 코드를 어떻게 구성할지 고려할 때 주관적인 절충이 이루어져야 한다는 것을 말하는 것입니다. 최근 인라인 스타일이 더 보편화된 사실은 SoC가 개발자들 사이에서 일부 지지를 잃고 있다는 징후입니다.

<br>

\## 결론

LoB는 코드베이스를 더 인간적이고 유지보수하기 쉽게 만들 수 있는 주관적인 소프트웨어 설계 원칙입니다. 다른 설계 원칙들과 절충되어야 하며, 코드 단위가 작성되는 시스템의 한계를 고려해야 하지만, 실용적인 한 이 원칙을 준수하면 소프트웨어의 유지보수성, 품질 및 지속 가능성이 향상될 것입니다.

번역 끝.

\*\*\*

<br>

\### 제 의견

코딩을 할 때 고민되는 부분입니다. SoC를 챙길지 DRY를 챙길지, LoB를 챙길지. 개념을 알아두면, 이런 고민거리 해결의 시작입니다!

개발원칙들간 충돌이 있다고 하지만, 프로젝트 규모에 따라 더 가치를 둘 개발원칙이 있다고 생각합니다.

예를 들면, OS 관련된 모든 코드를 하나의 파일에 넣어둘 순 없겠죠. 그러나 HTMX (리엑트와 비슷한 AJAX library)는 \[단 하나의 파일\]([https://github.com/bigskysoftware/htmx/blob/master/src/htmx.js)에](https://github.com/bigskysoftware/htmx/blob/master/src/htmx.js\)에) 코드를 몰아넣었습니다.

Django (MVT), Spring (MVC) 같은 기능이 많은 프로젝트는 SoC이 잘되어 있습니다. 그러나 flask, FastAPI같은 micro framework는 단 하나의 파일에도 작동합니다.

DRY를 위해 함수를 나누다보면 위의 LoB가 약해집니다. 함수를 많이 나눌수록 어떻게 나눴는지, 어떤 함수들이 있는지 내부 싱크가 필요할 거 같습니다. (개발자 교육을 통하든, 개발문서를 통하든)

DRY, SoC 대비 상대적으로 생소한 LoB도 SW 설계시 잘 챙겨나갔으면 좋겠습니다\\~