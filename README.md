# QuickStartVueJs
### 1. Vue.js?
- Angular, React에 비해 학습비용이 적음
- MVVM 패턴 (Model - View - ViewModel) : 애플리케이션 로직과 UI의 분리를 위해 설계된 패턴
- View는 HTML/CSS로 작성 / ViewModel은 View의 실제 논리 및 데이터 흐름을 담당
- View는 ViewModel만 알고 있으면 되며, 나머지는 신경쓰지 않아도 됨
- ViewModel의 상태 데이터만 변경하면 즉시 View에 반영
- [가상 DOM](https://blog.hanumoka.net/2018/08/15/web-20180815-web-virtual-dom/)을 지원하므로 아주 빠른 UI 랜더링 속도를 제공 
~~~
* vue.js 설치
  1. Node.js 설치 - [sudo npm install -g npm]
  2. Visual Studio Code 설치 - [http://code.visualstudio.com]
    - 플러그인 설치 (cmd + shift + x)
    - 플러그인 종류
      - view-in-browser : html 파일을 기본 브라우저로 볼 수 있음
      - vetur : vue.js 코드 문법강조, 코드 자동완성, 디버깅, 린팅 기능
      - HTML Snippets  : HTML 태그 조각을 빠르게 작성 가능
      - JS-CSS-HTML Formatters : JS, CSS, HTML 자동완성 / Ctrl + Space
      - Vue 2 Snippet : vue.js 2.0 코드 조각 지원과 문법 강조 기능
      - Vue-beautify : vue.js 코드 정리 배치 기능
      - ESLint : 자바스크립트 코드 스타일, 문법 체크 기능
   3. 크롬 브라우저 및 Vue.js devtools 설치 - 디버깅
   4. Vue-CLI 설치 - [sudo npm install -g yarn @vue/cli]
~~~
### 2. Vue.js 기초
1. 예제 분석
  ~~~
  <!DOCTYPE html>
  <html>
  <head>
      <meta charset="utf-8">
      <title>hello vue.js</title>
      <script src="https://unpkg.com/vue@2.5.16/dist/vue.js"></script>
  </head>
  <body>
      <div id="simple">
          <h2>{{message}}</h2>
      </div>
      <script type="text/javascript">
          var model = {
              message: '첫 번째 Vue.js 앱입니다.'
          };

          var simple = new Vue({
              el: '#simple',
              data: model
          })
      </script>
  </body>
  </html>
  ~~~
  * model 객체 : Model (데이터를 가짐)
  * simple 객체 : ViewModel(Vue 객체), el(HTML 요소), data(model 객체를 참조)
  * model(데이터)가 변경되면 그 즉시 뷰모델 객체는 HTML(뷰)에 반영
  * {{}} -> 콧수염 표현식(Mustache Expression)
  * MVVM 패턴
  ~~~
  #M(Model)
  var model = {
    message : "~~"
  }

  #V(View)
  <div id = "simple">
    <h2>{{message}}</h2>
  </div>

  #VM(ViewModel)
  var simple = new Vue({
  ~~
  })

  View (유저 인터페이스)    ->     ViewModel(상태와 연산)  ->   Model(도메인 특화 데이터)
                데이터바인딩과 커맨드                   업데이트 
  View (유저 인터페이스)    <-     ViewModel(상태와 연산)  <-   Model(도메인 특화 데이터)
                      알림 전송                      알림 전송

  ~~~
2. 기본 디렉티브
  * v-text, v-html 디렉티브
    - 선언적 렌더링을 위해 HTML 요소 내부 템플릿 표현식(콧수염 표현식)이외에 디렉티브라는 것을 이용해 표현할 수 있음
    ~~~
    Before) <h2>{{message}}</h2>
    After) <h2 v-text="message"></h2>
    ~~~
  * 








