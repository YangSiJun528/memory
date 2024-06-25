## FE/Web 역사
### GPT를 사용한 요약 + 추가 정리

- 초기 웹(1990년대 초반)
	- HTML의 등장(1991): 팀 버너스 리가 HTML을 개발하여 웹 페이지의 구조를 정의함.
	- 정적 웹사이트: 초기 웹 페이지는 텍스트와 이미지로만 구성된 정적 콘텐츠로 이루어짐.
- 첫 번째 전환기(1990년대 중반)
	- CSS의 등장(1996): 스타일 시트 언어로 웹 페이지의 스타일과 레이아웃을 정의할 수 있게 됨.
	- JavaScript의 등장(1995): 브렌던 아이크가 개발하여 웹 페이지에 동적 기능을 추가할 수 있게 함.
	- Flash(1996): 매크로미디어(후에 어도비)가 개발한 멀티미디어 플랫폼으로, 애니메이션, 비디오, 인터랙티브 콘텐츠를 웹 페이지에 삽입할 수 있게 함. Flash는 특히 2000년대 초반까지 웹 애니메이션과 게임에서 널리 사용됨.
- 웹 2.0 시대(2000년대 초반)
	- AJAX(2005): 비동기 JavaScript와 XML 기술을 사용하여 페이지를 다시 로드하지 않고도 데이터를 교환할 수 있게 함.
	- jQuery(2006): 크로스 브라우저 이슈를 해결하고, JavaScript를 더 간편하게 사용하게 해주는 라이브러리.
		- 편리한 DOM 조작 방식을 제공한다는게 장점
	- Flash의 전성기: 이 시기 Flash는 복잡한 애니메이션과 멀티미디어 콘텐츠를 웹에 제공하는 주요 도구로 자리잡음.
- 싱글 페이지 애플리케이션(SPA)와 프레임워크의 등장(2010년대 초반)
	- 요 즈음에 웹 서비스가 복잡해지고 모바일 기기가 대중화 되면서 app-like 웹이 필요해짐.
		- 웹 페이지 개발이 복잡해짐에 따라 10년대 중반 즈음부터 FE라는 직군이 생기기 시작함.
	- AngularJS(2010): 구글이 개발한 프레임워크로, MVC 패턴을 통해 구조화된 애플리케이션을 개발할 수 있게 함.
		- 제로초의 영상에서 MVC가 FE 개발과 잘 맞는 패턴은 아니라는 의견이 나옴
	- React(2013): 페이스북이 개발한 라이브러리로, 컴포넌트 기반 개발 방식을 도입하여 효율적인 UI 구축을 가능하게 함.
	- Vue.js(2014): 점진적으로 적용 가능한 프레임워크로, 사용자 정의 컴포넌트와 반응형 데이터 바인딩을 특징으로 함.
	- Flash의 퇴조: HTML5와 CSS3의 등장으로 브라우저 자체에서 멀티미디어와 애니메이션을 지원하게 되면서 Flash의 사용이 급격히 감소.
		- SEO 지원을 하지 못하고, 보안성이 낮은 것도 문제였음.
	- 웹 컴포넌트: 재사용 가능한 캡슐화된 컴포넌트를 만들 수 있는 표준 기술.
- 최신 웹 개발 동향(2020년대 초반)
	- 모던 JavaScript(ES6 이후): 화살표 함수, 클래스, 모듈 등 새로운 문법과 기능이 도입되어 개발 생산성이 크게 향상됨.
	- TypeScript: JavaScript의 슈퍼셋 언어로, 정적 타입 검사 기능을 제공하여 코드의 안정성을 높임.
	- 프레임워크의 발전: Next.js(React 기반)와 Nuxt.js(Vue.js 기반)와 같은 서버사이드 렌더링(SSR)과 정적 사이트 생성(SSG)을 지원하는 프레임워크가 인기를 끔.
	- Svelte(2016): Rich Harris가 개발한 새로운 프론트엔드 프레임워크로, 빌드 타임에 컴파일되는 방식을 채택하여 런타임 성능을 최적화함. Svelte는 가벼운 번들 크기와 뛰어난 성능으로 주목받고 있음

- web + js 관련 역사
	- 역사
		- 동적인 기능(DOM 조작 등)을 제공하기 위해 브라우저에 사용할 프로그래밍 언어인 JS 등장
			- 브라우저마다 호완성이 달라 구현이 어려운 문제를 해결하고 쉬운 기능들을 제공하는 라이브러리 JQuery 등장
		- 새로운 데이터 요청 시 모든 페이지 업데이트가 발생하는 비효율을 제거하기 위해 라이브러리 AJAX 등장
		- SNS와 같이 App-Like 페이지의 요구로 인한 동적 페이지의 필요성 증가. DOM 조작 시 비효율을 줄이기 위해 SPA(가상 DOM)이 등장
		- SPA(CSR)는 브라우저의 자원을 상대적으로 많이 사용하고, SEO(검색 엔진 최적화)가 어려운 문제를 해결하기 위해 SSR 등장
		- 효율적으로 DOM을 조작하는 Svelte의 등장
	- 생각해볼 만한 포인트
		- 결론적으로 모든 기술은 기존의 기술의 문제를 해결하기 위해서 등장했다. 
			- 즉, 그 기술의 배경인 해결하고자 하는 문제에 해당되지 않는다면 굳이 사용할 필요는 없다고 생각한다. 기술 스택이나 그런 것도 고려해야 하겠지만...
		- FE와 BE의 분리가 꼭 필요한가?
			- 단순한 페이지라면 NextJS같은 풀스택 프레임워크나 JQuery+supabase 같은 기술 스택이 나을 수도 있음.
				- JQuery는 그래도 이제 크로스브라우징 문제도 없고 바닐라 JS의 지원이 커서 굳이 사용할 정도인가 싶긴 한데, 아무튼 선택 못할 정도는 아니라고 생각함.
				- 특히 카카오나 배인의 기술 블로그가 [워드프레스](https://www.youtube.com/watch?v=eFl7ysjQMJQ)기반이라는 걸 생각하면 문제를 해결하는데 적절한지 고려해야 하지, 기술에 직찹하는 것은 좋지 않아보인다.

- FE 기준이 아니라 BE, FE를 합친 기준
	- 참고: https://youtu.be/EwY6hbAxdV8?si=1dLQewlTinU40XWn
	- 정적 페이지: HTML 순수 개발
	- 동적 페이지: WAS(JSP 등) 기반 개발, SSR 방식
	- AJAX의 등장: CSR의 등장, FE와 BE 역할의 분리
	- 다시 SSR 필요: CSR의 SEO 문제, 그러나 불편한 SSR 구현 방법
	- Nest.js와 같은 풀스택 프레임워크의 등장: 불편한 SSR 구현/서버 띄우기 등, 여러 불편함(니즈)을 해결하는 풀스템 프레임워크 등장. FE 개발자가 Server-Side를 사용하기 쉽게 해줌. (Next.js는 React와 호환됨)

### 참고할만한 것들

- https://yozm.wishket.com/magazine/detail/1289/
- https://velog.io/@minsangk/%EC%A7%A7%EA%B2%8C-%EC%8D%A8%EB%B3%B4%EB%8A%94-%EC%9B%B9-%ED%94%84%EB%A1%A0%ED%8A%B8%EC%97%94%EB%93%9C%EC%9D%98-%EC%97%AD%EC%82%AC
- https://www.youtube.com/watch?v=sG1YA6IssTY&list=PLg9hpcQTPCpd4yX6rJin8mt6uVhsD_cj8
- https://www.youtube.com/watch?v=We9W395EUmw
- 리액트 관련: https://www.youtube.com/watch?v=WRvd29xF8dc

- 2024 FE 무료 강의: https://www.youtube.com/watch?v=Yv5tSNr4h2c&t=18093s
- 번들링(Bundling - React - vite): https://www.youtube.com/watch?v=iX3Nu1FcZKA
- 얄코 - FE 관련 영상도 있음 - https://www.youtube.com/watch?v=c5QFSvty8nc&list=PLpO7kx5DnyIGzsb-4N9Z76RIR1xfprlIl&index=12
	- DOM이나 AJAX 같은거

## Bundling

- 빌드 시 필요한 코드만 가져와서 하나의 js로 통합하는 걸 Bundling이라고 함 
- 여러 라이브러리가 지원하는데, 번들링 외에 여러 기능을 제공함.
	- CRA, Vite, Parcel 등

- 주로 제공하는 기능들
	- 프로젝트 시작 구조 생생
	- 번들링: 빌드 시 필요한 코드만 가져와서 하나의 js로 통합하는 역할
	- 개발 서버: 실행하는 환경

번들링(Bundling - React - vite): https://www.youtube.com/watch?v=iX3Nu1FcZKA

## DOM & CSSOM & Virtual DOM

DOM(Document Object Model)과 CSSOM(CSS Object Model)

- DOM & CSSOM는 Web API를 기반으로 만들어진 자바스크립트 객체(자바스크립트가 아니여도 됨, API니까)
	- Web API를 사용해서 DOM이나 CSSOM을 조작할 수 있다.
	- Tree 형태를 가진다. 그래서 이름 뒤에 Tree가 붙기도 함.
- 설계도 역할을 하는 문서 HTML(DOM), CSS(CSSOM) 정보를 기반으로 생성된 객체
- DOM Tree + CSSOM Tree을 합쳐서 실제로 사용자에게 보여지는 역할의 Render Tree 생성
	- Render Tree는 보여져야 하는 부분만 구성된다. (`display:none` 같은거 있으면 포함 안함) 

#### Virtual DOM vs DOM
- Virtual DOM: DOM을 흉내내는 가벼운 객체
	- DOM 변경을 메모리에서 먼저 처리하여 실제 DOM 조작 횟수를 줄임.
    - 변경 사항을 효율적으로 추적하고 필요한 부분만 업데이트.
    - Vue, React에서 사용
    - 이전에 JQuery나 Web API(순수 JS)에서는 DOM의 특정 node를 조작하면 하위 node에도 영향이 가는 문제를 해결하기 위해, 미리 효율적인 부분을 하고 그 부분만 변경.

(Angular는 V DOM없이 다른 방식인거 같은데, 잘 안쓰니까 따로 안알아봄)

- Svelte(DOM) vs React,Vue(V DOM)
	- React,Vue는 라이브러리(아님 프레임워크), 사용자 브라우저에서 가상 돔을 가지고 효율적으로 업데이트 함.
		- V DOM을 사용하고, 라이브러리 코드가 동작(diff 확인)하면서 사용자 브라우저 부하
	- Svelte는 컴파일러, 컴파일 시점에 어떤 식으로 DOM을 효율적으로 조작할지 미리 컴파일 함.
		- 컴파일을 통해 최소한의 코드, 효율적인 방식으로 직접 DOM이 조작되므로 React나 Vue보다 빠름.

- https://www.youtube.com/watch?v=07Lppn7UdIs
	- 얄코 << 추천
- https://www.youtube.com/watch?v=gc-kXt0tjTM
- https://www.youtube.com/watch?v=1ojA5mLWts8
	- Svelte와 React의 차이

## AJAX

Ajax(Asynchronous JavaScript and XML, 에이잭스) 초기에는 XML을 사용해서 이름이 이렇지만, 실제로는 다른 형식도 가능하고 요즘은 JSON을 많이 씀.

> 이번 영상에서는 AJAX의 개념과 작동원리, 구현방법에 대해 알아봅니다. 
> AJAX는 웹페이지에서 필요한 정보만 서버로부터 받아와 화면을 새로 고침 없이 업데이트하는 기술입니다. AJAX가 없던 시대에는 사용자의 모든 행동마다 화면이 깜빡이면서 새로 리로드되었고, 이는 사용자의 불편함과 네트워크 트래픽 낭비를 초래했습니다. 
> 하지만 AJAX의 등장으로 페이지 전체가 아닌 필요한 부분만을 동적으로 업데이트할 수 있게 되어 사용성이 크게 향상되었습니다. 
> AJAX의 구현방법으로는 XMLHTTPRequest와 Fetch API가 대표적입니다. 
> 두 API 모두 웹 브라우저에서 제공하는 Web API의 일부로서, 따로 라이브러리를 설치하지 않고 사용할 수 있습니다.  
> AJAX는 웹 개발에서 중요한 기술로, 사용자 경험을 향상시키고 네트워크 트래픽을 줄이는 데 큰 기여를 합니다.

다만 보여지는 화면을 동적으로 업데이트하는 기능은 DOM 조작으로 한다고 봐야 함. 

- https://youtu.be/c5QFSvty8nc?si=W9fQ88bW7tnU9Yow
	- 얄코 << 추천
