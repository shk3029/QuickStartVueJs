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
		- 단방향 디렉티브 (HTML 요소에서 값을 변경하여도 모델 객체의 값은 안바뀜)
    ~~~
    Before) <h2>{{message}}</h2>
    After) <h2 v-text="message"></h2>
    ~~~
      - v-text, {{ }} : innerText 속성에 연결, 태그 문자열을 HTML 인코딩하여 나타내기 때문에 화면에는 태그 문자열이 그대로 나타남
        ~~~
        > model.message = "<i>HTML 태그는 어찌 되나?</i>"; 
        > document.getElementById("simple").innerHTML
        ->> "<h2>&lt;i&gt;HTML 태그는 어찌 되나?&lt;/i&gt;</h2>"
        ~~~
      - v-html : innerHTML 속성에 연결, 문자열을 파싱하여 화면에 나타냄 
        ~~~
        > model.message = "<i>HTML 태그는 어찌 되나?</i>"; 
        > document.getElementById("simple").innerHTML
        ->> "<h2><i>HTML 태그는 어찌 되나?</i></h2>"
        ~~~
    - v-html은 script 태그를 그대로 바인딩하므로 XSS(Cross Site Scripting) 공격에 취약함, 꼭 필요한 경우가 아니면 v-text 사용을 권장
	* v-bind 디렉티브
		- 요소 객체의 컨텐트 영역이 아닌 속성들을 바인딩
		- 단방향 디렉티브 (HTML 요소에서 값을 변경하여도 모델 객체의 값은 안바뀜)
		~~~
		<input id="a" type="text" v-bind:value="message">
		<br/>
		<img v-bind:src="imagePath" />
		
		<script>
		 var model = {
					message: 'v-bind 디렉티브.',
					imagePath: "http://sample.bmaster.kro.kr/photos/61.jpg"
			};
		~~~
		- v-bind:src에서 v-bind를 생략하고 :src로 사용가능
	* v-model 디렉티브
		- 양방향 디렉티브 : 요소에서 변경한 값이 모델 객체에 반영이 됨
		~~~
		<body>
				<div id="simple1">
						<div> 좋아하는 과일을 모두 골라주세요. </div>
						<input type="checkbox" value="1" v-model="fruits"> 사과,
						<input type="checkbox" value="2" v-model="fruits"> 키위,
						<input type="checkbox" value="3" v-model="fruits"> 포도,
						<input type="checkbox" value="4" v-model="fruits"> 수박,
						<input type="checkbox" value="5" v-model="fruits"> 참외,
				</div>
				<div id="simple2">
						선택한 과일들 : <span v-html="fruits"></span>
				</div>
				<script type="text/javascript">
						var model = {
								fruits: []
						};

						var simple1 = new Vue({
								el: '#simple1',
								data: model
						});

						var simple2 = new Vue({
								el: '#simple2',
								data: model
						});
				</script>
		</body>
		</html>
		~~~
		- 하나의 모델을 2개의 Vue 객체에서 참조 
		- 사용자 입력 값을 뷰모델 객체를 통해 즉시 변경
		- v-model 디렉티브 수식어
			* lazy (v-model.lazy="name") : 입력폼에서 이벤트가 발생할 때, 입력한 값을 데이터와 동기화 
			* number : 숫자가 입력된 경우 number 타입의 값으로 자동 형변환
			* trim : 문자열 앞뒤 공백을 자동제거
	* v-show, v-if, v-else, v-else-if 디렉티브
		1. v-if : v-show 디렉티브와 비슷한 기능, 렌더링 여부 차이가 있음
			- v-if 디렉티브는 조건에 부합되지 않으면 렌더링을 하지 않음
			- v-show는 일단 HTML 요소를 렌더링 한 후, display 스타일 속성으로 보여줄지 여부를 결정
			- 따라서, 자주 화면이 변경되는 부분에서는 v-if 디렉티브보다는 v-show 디렉티브를 사용하는 것이 바람직함
		2. 예제
		~~~
		<div id="account">
			잔고 : <input type="text" v-model="balance" />
			<br/> 회원 등급 : <span v-if="balance >= 1000000">Gold</span>
			<span v-else-if="balance >= 500000">Silver</span>
			<span v-else-if="balance >= 200000">Bronze</span>
			<span v-else>Basic</span>
    </div>
    <script type="text/javascript">
        var simple = new Vue({
            el: "#account",
            data: {
                balance: 0
            }
        })
    </script>
		~~~

















