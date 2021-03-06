# vue.js-framework-development-guide

vue.js 프레임워크 개발 가이드 ver 0.1.1





## 목적

* 신규 프로젝트를 시작하면서 개발 팀원의 개발 환경 설정과 코딩 스타일을 통일시켜 더 나은 코드 품질을 얻기 위한 가이드





## 목차

* [vue.js 프레임워크 도입](#vue.js-프레임워크-도입)
* [개발 환경 설정](#개발-환경-설정)
* [Vue CLI를 사용하여 싱글 페이지 애플리케이션 구조 생성](#Vue-CLI를-사용하여-싱글-페이지-애플리케이션-구조-생성)
* [생성된 프로젝트를 가져오기](#생성된-프로젝트를-가져오기)
* [production 리소스 빌드 및 미리보기](#production-리소스-빌드-및-미리보기)
* [활용 기술 정리](#활용-기술-정리)
* 코딩 컨벤션
* vue.js 컴포넌트 스타일 가이드
* [문제점해결](#문제점해결)
* [참고자료](#참고자료)





## vue.js 프레임워크 도입

* 프로젝트 메인 페이지의 사용자 경험 및 퍼포먼스 향상을 위해서 도입 결정
* 한마디로 페이스북 같은 피드(목록) 처리가 필요함. SPA로 구현
* 백엔드를 순수 API 서버로만 사용하기 위해서 프론트엔드 프레임워크 도입
* 왜 vue?
  - No JSX. html 템플릿만 사용함
  - react, angular 프레임워크에 비해 러닝 커브가 낮음





## 개발 환경 설정

* NPM (Node Package Manager) 사용을 위해 [Node.js Stable 버전](https://nodejs.org/en/ ) 설치
* NPM을 통해 손쉬운 모듈 세팅이 가능해짐. 대부분의 문서가 NPM을 기준으로 작성되어 있다.
* 프론트엔드 개발이 가능한 IDE 설치. [Visual Studio Code](https://code.visualstudio.com/download )(이하 VSC) 추천. 확장기능 설치가 간편하고 java 스프링부트도 테스트 및 개발 가능
* 크롬브라우저에서 Vue 개발자 도구 설치.
  - 크롬 웹 스토어에서 vue를 검색해서 Vue.js devtools 를 설치하자.		





## Vue CLI를 사용하여 싱글 페이지 애플리케이션 구조 생성

* vue-cli 3 기준
* 이전 vue-cli 2 패키지 제거 
  ```bash
  $ npm r -g vue-cli
  ```
  - r : remove, -g : global, i : install
  
* vue-cli 3 전역 설치
  ```bash
  $ npm i -g @vue/cli
  ```
* vue 버전이 3.x 이상인지 체크
  ```bash
  $ vue --version
  ```

* 새로운 프로젝트 생성
  ```bash
  $ vue create {프로젝트명:hello-vue-cli}
  ```
  - 설정 default (babel, eslint) 선택
  - 자주 사용하는 플러그인 추가
    ```bash
    $ vue add router
    ```
  - 최종 생성되는 프로젝트 구조
    ``` html
    |- /dist 				--- build 후 결과물
    |- /node_modules 		--- 모듈 패키지
    |- /public				--- 번들링되지 않는 ROOT 폴더
        |- favicon.ico
        |- index.html
    |- /src				
        |- /assets			--- js, css, image(jpg, png, svg) 등의 asset
        |- /components		--- 단일 파일 컴포넌트
        |- /views			--- 라우터 페이지 컴포넌트
        |- App.vue			--- 애플리케이션 메인 컴포넌트
        |- main.js			--- 애플리케이션 메인 스크립트
        |- router.js		--- 라우터 메인 스크립트
    |- babel.config			--- 바벨 트랜스파일러 플러그인 설정
    |- package.json			--- npm 의존성 정의 파일 (중요!)
    |- package-lock.json	--- 
    |- README.md			--- Vue CLI 자동 생성 docs
    ```
  





## 생성된 프로젝트를 가져오기

* 생성된 프로젝트를 VSC로 import하여 작업 영역 설정
  - vue framework를 구성하는 방법은 여러가지가 있으나 컴포넌트 재사용 및 확장을 위해 싱글 파일 컴포넌트 방식을 사용한다. (*.vue)
  
  - 싱글 파일 컴포넌트는 그 자체로는 브라우저에서 사용할 수 없고 빌드 도구를 통한 컴파일 과정이 필요하다
  
  - 빌드 도구는 대중적인 webpack 4를 사용. vue-cli ver.3을 설치하였다면 이미 프로젝트에 포함되어 있다.
  
  - javascript ES6 > ES5 트랜스파일링을 위해 babel-plugin을 포함시킨다
  
  - 작업 영역 ROOT 폴더에서 내부 터미널 실행
    ```bash
    ctrl + ` (VSC 기준)
    ```
    
  - 개발 서버 구동
    ```bash
    $ npm run serve
    
    # result
    App running at:
    - Local:   http://localhost:8080/
    - Network: http://192.168.137.1:8080/
    
    Note that the development build is not optimized.
    To create a production build, run npm run build.
    ```
    
  - 프로젝트 폴더 하위에 node_modules 폴더가 없을 경우 node_modules missing 오류가 발생
    
    ```bash
    $ npm run serve
    
    # result
    > vue-cli-service serve
    
    'vue-cli-service'은(는) 내부 또는 외부 명령, 실행할 수 있는 프로그램, 또는
    배치 파일이 아닙니다.
    npm ERR! code ELIFECYCLE
    npm ERR! errno 1
    npm ERR! hiclass-vue@0.1.0 serve: `vue-cli-service serve`
    npm ERR! Exit status 1
    npm ERR!
    npm ERR! Failed at the hiclass-vue@0.1.0 serve script.
    npm ERR! This is probably not a problem with npm. There is likely additional logging output above.
    npm WARN Local package.json exists, but node_modules missing, did you mean to install?
    ```
    
  - 필요 패키지 설치 필요.
    
    ```bash
    $ npm install
    ```
    
  - 최종 결과물 빌드
    ```bash
    $ npm run build
    ```
    
  - 웹팩을 통해 모든 소스파일을 번들링한다.
  
  - 프로젝트 폴더 하위에 생성된 dist 폴더의 모든 파일을 웹서버에 포함시킨다.
  
  - 백엔드 서버를 기동하여 서비스한다.	
  





## production 리소스 빌드 및 미리보기

* 빌드
  ```bash
  $ npm run build
  ```
  - 번들링한 production 정적 리소스는 탐색기의 file:// 경로로 확인할 수 없다
  - 간단한 node serve 패키지로 미리보기 할 수 있다. [serve](https://www.npmjs.com/package/serve) 로컬서버 패키지를 설치
  
* serve 설치
  ```bash
  $ npm install -g serve
  ```
  
* 기동방법
  ```bash
  $ serve -s {리소스 폴더:dist}
  ```
  
* 5000 포트로 기동
  
  ```bash
  $ serve -s dist
  ```
  8080 포트로 기동
  
  ```bash
  $ serve -s dist -l 8080
  ```
  
* [localhost:5000](http://localhost:5000) 또는 [localhost:8080](http://localhost:8080) 주소로 접속





## 활용 기술 정리

* webpack = 빌드 도구. 앱 번들링
  - 
* 코드 스플리팅
  - 
* babel = 자바스크립트 트랜스파일링
  - convert ES6(ES2015) > ES5
* nuxt.js = 서버 사이드 렌더링 (SEO 추후 고려)
  - 
* vue-router = SPA 라우트
  - 
* vuex = 상태 관리
  - <https://vuex.vuejs.org/kr/guide/state.html>





## 코딩 컨벤션

#TBA





## Vue.js 컴포넌트 스타일 가이드

#TBA





## 문제점해결

* ‘Promise’이(가) 정의되지 않았습니다. (IE 브라우저 등)
  - ajax 요청을 위해 axios 라이브러리를 사용하는데 해당 ES6 JS문법을 IE가 지원하지 않음. es6-promise를 적용한다.
  - npm 패키지 설치
    ```bash
    $ npm install es6-promise --save
    ```
  - main.js 파일 수정. axios 로딩 전 추가
    ```javascript
    // ES6 방식 #babel 트랜스파일러 사용시
    import 'es6-promise/auto
    ```

* error: Unexpected console statement (no-console)

  * [disallow the use of `console` (no-console)](<https://eslint.org/docs/rules/no-console.html>)






## 참고자료

* vue core
  
  - **[Vue.js ver 2.x 공식 가이드(한글)](https://kr.vuejs.org/v2/guide/ )**
  
* vue scaffolding
  - **[Vue CLI 3 사용법](http://www.daleseo.com/vue-cli3/ )**
  - [Vue Cli 3 공식가이드](<https://cli.vuejs.org/guide/>)
  - [Vue CLI 3.0 is here! by Evan You](https://medium.com/the-vue-point/vue-cli-3-0-is-here-c42bebe28fbb )
  
* vue plugin
  - [뷰 라우터](https://joshua1988.github.io/vue-camp/vue/router.html )
  - [[vue-router] SPA 방식 애플리케이션 운영 시 주의사항](https://jamong-icetea.tistory.com/214 )
  - [babel browserslist](https://cli.vuejs.org/guide/browser-compatibility.html#browserslist )
  - [Vuex : 상태 관리 패턴 + 라이브러리](<https://vuex.vuejs.org/kr/>)

* build Tool

  - **[webpack : 웹 애플리케이션 번들러](https://poiemaweb.com/es6-babel-webpack-2 )**

* Javascript Transpiler

  - [babel : 자바스크립트 트랜스파일러](https://poiemaweb.com/es6-babel-webpack-1 )

* Lint

  - [babel-eslint github](https://github.com/babel/babel-eslint )

* Code Convention

  - **[Vue.js 컴포넌트 스타일 가이드](<https://github.com/pablohpsilva/vuejs-component-style-guide/blob/master/README-KR.md>)**

  

* [Vue + SpringBoot 개발환경 구축](https://handcoding.tistory.com/196 )

* [Vuejs로 모바일 웹 구축하기 - ZUM](https://zuminternet.github.io/ZUM-Pilot-vuejs/ )	

* [[Vue] 개발환경 만들기 (without vue-cli)](https://velog.io/@kyusung/Vue-app-sfc-without-vue-cli )

* [액시오스 HTTP 통신 라이브러리](https://joshua1988.github.io/vue-camp/vue/axios.html )

* [[Vuetorials] 1. @vue-cli 3.0](https://jaeyeophan.github.io/2018/10/21/Vuetorials-1-vue-cli-3-0/ )

* [Awesome Vue.js github](<https://github.com/vuejs/awesome-vue>)

* [Axios github](<https://github.com/axios/axios>)

* [프론트 엔드 가이드(Grab Front End Guide) 번역](<https://m.blog.naver.com/magnking/221149133410>)

* [Vue CSS Tutorial - Class and Style Binding](<https://coursetro.com/posts/code/136/Vue-CSS-Tutorial---Class-and-Style-Binding>)
