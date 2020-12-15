# QuickStartVueJs (2019/2/10 ~)
- [Part1. 시작하기](#part1)
  - Vue.js 란?
  - 개발 환경 설정
- [Part2. Vue.js 기초](#part2)
  - 기본 디렉티브
  - 반복 렌더링 디렉티브
  - 기타 디렉티브
  - 계산형 속성
- [Part3. Vue 인스턴스](#part3)
  - el, data, computed 옵션
  - 메서드
  - 관찰 속성
  - v-cloak 디렉티브
  - Vue 인스턴스 라이프 사이클
- [Part4. 이벤트 처리](#part4)
  - 인라인 이벤트 처리
  - 이벤트 핸들러 메서드
  - 이벤트 객체
  - 기본 이벤트
  - 이벤트 전파와 버블링
  - 이벤트 수식어
    - once 수식어
    - 키코드 수식어
    - 마우스 버튼 수식어
- [Part5.스타일](#part5)
  - 스타일 적용
  - 인라인 스타일
  - CSS 클래스 바인딩
  - 계산형 속성, 메서드를 이용한 스타일 적용
  - 컴포넌트에서의 스타일 적용
  - TodoList 예제
## Part1
### Part1. Vue.js?
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

## Part2
### Part2. Vue.js 기초
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
3. 반복 렌더링 디렉티브
	* v-for 디렉티브
		1. 데이터가 배열형태
		~~~
		<tr v-for="contact in contacts"></tr>
		* 만약 인덱스틀 이용할 경우
		<tr v-for="(contact,index) in contacts"></tr>
		~~~
		2. 데이터가 객체형태
		~~~
		<tr v-for="(val, key) in contacts"></tr>
		* 만약 인덱스를 이용할 경우
		<tr v-for="(val, key, index) in contacts"></tr>
		~~~
		3. v-for, v-if를 함께 사용할 경우 주의
			* 적용 순서 : v-for 디렉티브가 먼저 수행되고 v-if가 적용됨
			~~~
			<tr v-for="contact in contacts" v-if="contact.address.indexOf('서울') > -1"></tr>
			~~~
	* 하나의 요소가 아닌, 여러 요소의 그룹을 반복 렌더링하려면?
		- template 태그를 사용
	* key 특성(Attribute)
		- DOM 요소의 위치를 직접 변경하고자 한다면,
		- key 특성에 기본키(Primary key) 값을 바인딩
		~~~
		<template v-for="(contact, index) in contacts">
			<tr :key="contact.no">
				<td>{{contact.no}}></td>
				<td>{{contact.name}}></td>
				<td>{{contact.tel}}></td>
			</tr>
		</template>
		~~~
		- 일반적으로 key 특성을 바인딩할 것을 권장하지만, 렌더링 속도가 개선된다고 말할 수는 없음
		- v-for 디렉티브는 주로 배열의 데이터를 출력할 것인데, 배열 데이터가 변경될 때 추적이 되지 않는 작업이 있음
			1. 배열 데이터를 인덱스 번호로 직접 변경할 경우
				- ex) list.contact[0] = {no:12, name:"12" ~~~}
				- 이렇게 콘솔에서 변경해도 아무변화가 없음
				- 하지만 속성을 변경할 경우, Vue 인스턴스 내부의 감시자가 추적해내기 때문에 즉시 변경됨 (list.contact[0].name = 'js')
				- 기존의 배열 값을 직접 변경하기 위해서는 Vue.set(list.contact, 0, {no:12, name="12"~~~}로 변경해야함
4. 기타 디렉티브
	* v-pre : HTML 요소에 대한 컴파일을 수행하지 않음
	* v-once : HTML 요소를 단 1번만 렌더링 (콘솔로 값을 변경하여도 안바뀜) - 초기값이 주어지면 변경되지 않는 UI를 만들 때 사용할 수 있음
5. 계산형 속성
	* Vue 객체의 computed 속성과 함께 함수로 등록해두면 마치 속성처럼 사용할 수 있음
	~~~
	<div id="example">
		<input type="text" v-model="num" /><br/> 1 ~ n 입력된 수의 합 : <span>{{sum}}</span>
	</div>
	<script type="text/javascript">
		var vmSum = new Vue({
		el: "#example",
		data: {
			num: 0
		},
		computed: {
			sum: function() {
					var n = Number(this.num);
					if (Number.isNaN(n) || n < 1) return 0;
					return ((1 + n) * n) / 2;
			}
		}
		})
	</script>
	~~~

	* 주의사항
		1. 함수 내부의 this
			- 함수 안의 this는 Vue 객체 자신을 참조
			- 함수 내부에서 다른 콜백 함수를 실행했을 때 this가 다른 값으로 연결될 수 있으므로 주의
		2. 데이터 타입
			- HTML 요소 내부에서는 모두 문자열로 다루어지므로, Number(), parseInt() 함수를 이용해 명시적으로 숫자 값을 변환해주어야 한다.


## Part3
### Part3. Vue 인스턴스
1. el, data, computed 옵션
- data
  - data 옵션에 주어진 모든 속성들은 Vue 인스턴스 내부에서 직접 이용되지 않음
  - Vue 인스턴스와 Data 옵션에 주어진 객체 사이에 프록시(대리인)를 두어 처리
  - data 옵션은 Vue 인스턴스가 관찰하는 데이터 객체를 의미 (변경사항은 즉시 감지)
  - vm.$data.name
- el
  - Vue 인스턴스에 연결할 HTML DOM 요소를 지정
  - 여러 개 요소에 지정할 수 없음
  - el 옵션은 Vue 인스턴스를 생성할 때 미리 지정할 것을 권장
  - Vue 인스턴스가 HTML 요소와 연결되면 도중에 연결된 요소를 변경할 수 없음
- computed
  - 이 옵션에서 지정한 값은 함수였지만, Vue 인스턴스는 프록시 처리하여 마치 속성처럼 취급
  - vm.$options.computed.sum
  - get / set function
2. 메서드
- Vue 인스턴스에서 사용할 메서드를 등록하는 옵션
- Vue 인스턴스를 이용해 직접 호출하거나, 디렉티브 표현식, 콧수염 표현식으로 사용
  ~~~
  <input type="text" v-model="num"/>
  1부터 입력된수의 합 : <span>{{sum()}}</span>

  var vm = new Vue({
    el : "#example",
    data : { num : 0},
    methods : {
      sum : function() {
        var n = Number(this.num);
        if(Number.isNaN(n) || n<1) return 0;
        return ((1+n) * n) / 2;
      }
    }
    })
  ~~~
- 계산형 속성과 메서드의 차이
  - 최종적인 결과물은 같아보이지만, 내부 작동 방식에 차이가 있음
  - 계산형 속성은 종속된 값에 의해 결과값이 캐싱이 됨
  - 따라서, vm.num이 같은 값이 들어왔을 때, 계산형 속성은 값을 바로 리턴하지만, 메서드는 매번 수행
- 메서드 작성 시 주의할 점
  - ECMAScript6가 제공하는 <U>화살표 함수</U>를 사용해서는 안됨
    - Arrow function(화살표 함수)은 function 키워드 대신 화살표(=>)를 사용하여 간략한 방법으로 함수를 선언하는 방법
    - 화살표 함수 내부에서는 this가 Vue 인스턴스를 가리키지 않고, 전역 객체를 가르킴
    - 메서드 내부에서는 보통 데이터 속성을 이용하기 때문에 this가 바뀌면 Vue 인스턴스 내부 데이터에 접근할 수 없음
3. 관찰 속성
- 계산형 속성 : 하나의 데이터 기반으로 다른 데이터를 변경할 필요가 있을 때 사용
- 관찰 속성 : 주로 긴 처리 시간이 필요한 비동기 처리에 적합하다는 특징을 가짐
  ~~~
  <body>
    <div id="example">
        x : <input type="text" v-model="x" /> <br/>
        y : <input type="text" v-model="y" /> <br/>
        덧셈 결과 : {{sum}}
    </div>
    <script type="text/javascript">
        var vm = new Vue({
            el: "#example",
            data: {
                x: 0,
                y: 0,
                sum: 0
            },
            watch: {
                x: function(v) {
                    console.log("## x 변경");
                    var result = Number(v) + Number(this.y);
                    if (isNaN(result)) this.sum = 0;
                    else this.sum = result;
                },
                y: function(v) {
                    console.log("## y 변경");
                    this.y = v;
                    var result = Number(this.x) + Number(v);
                    if (isNaN(result)) this.sum = 0;
                    else this.sum = result;
                }
            }
        })
    </script>
  </body>
  ~~~
- function(v) {...}는 x속성 또는 y속성이 변경될 때 호출되는 함수
- 하지만 이렇게 하면 값이 바뀔 때마다 매번 함수가 호출된다는 단점이 있음
- 이런 경우 계산형 속성이 더 효과적임
  ~~~
  <body>
    <div id="example">
        x : <input type="text" v-model="x" /> <br/> y : <input type="text" v-model="y" /> <br/> 덧셈 결과 : {{sum}}
    </div>
    <script type="text/javascript">
        var vm = new Vue({
            el: "#example",
            data: {
                x: 0,
                y: 0
            },
            computed: {
                sum: function() {
                    var result = Number(this.x) + Number(this.y);
                    if (isNaN(result)) return 0;
                    else return result;
                }
            }
        })
    </script>
  </body>
  ~~~
- 이렇게 작성하면 sum이 참조될 때만 해당 함수가 호출
- 그렇다면 언제 관찰 속성이 유용할까?
  - 긴 시간이 필요한 비동기 처리가 필요할 때
  - 가장 대표적인 예는 외부 서버와의 통신 기능
  - wath_Asynchronous(4) 예제 참고
  - 이 예제는 계산형 속성으로 구현할 수 없음
  - 계산형 속성은 값을 리턴해야 하기 때문에 동기적 처리만 수행이 가능함
  - 비동기 처리가 필요한 경우 관찰 속성이나 이벤트 처리 방법을 적용
4. v-cloak 디렉티브
- 만약에 템플릿에 머스태쉬 코드를 작성했더라면 머스태쉬 태그가 그대로 나타났을겁니다.
- 자바스크립트가 실행 되기전 즉, Vue 인스턴스가 제대로 준비되기 전 까지 우리의 템플렛을 위한 HTML 코드를 숨기고 싶을 때 사용
5. Vue 인스턴스 라이프 사이클
- Vue 인스턴스는 객체로 생성되고, 데이터에 대한 관찰 기능을 설정하는 등의 작업을 위해 초기화를 수행
- 다양한 라이프 사이클 훅 메서드를 적용할 수 있음
- Vue 컴포넌트를 만들고 관리할때 꽤 유용
- [라이프 사이클 훅](https://vuejs.org/v2/guide/instance.html)
  - beforeCreate : Vue 인스턴스가 생성되고 데이터에 대한 관찰 기능 및 이벤트 감시자 설정 전에 호출됨
  - created : Vue 인스턴스가 생성된 후에 데이터에 대한 관찰 기능, 계산형 속성, 메서드, 감시자 설정이 완료된 후에 호출
  - beforeMount : 마운트가 시작되기 전에 호출
  - mounted : el에 vue 인스턴스의 데이터가 마운트 된 후에 호출
  - beforeUpdate : 가상 DOM이 렌더링, 해치되기 전에 데이터가 변경될 때 호출, 이 훅에서 추가적인 상태 변경을 수행할 수 있음, 하지만 추가로 다시 렌더링을 하지는 않음
  - updated : 데이터의 변경으로 가상 DOM이 다시 렌더링되고 패치된 후에 호출, 이 훅은 호출되었을 때는 이미 컴포넌트 DOM이 업데이트가된 상태, DOM에 종속성이 있는 연산을 이 단계에서 수행할 수 있음
  - beforeDestroy : Vue 인스턴스가 제거되기 전에 호출
  - destroyed : Vue 인스턴스가 제거된 후에 호출, 이 훅이 호출될 때는 Vue 인스턴스의 모든 디렉티브의 바인딩이 해제되고, 이벤트 연결도 모두 제거됨

## Part4
### Part4. 이벤트 처리
1. 인라인 이벤트 처리
- 이벤트는 v-on 디렉티브를 이용해서 처리
- Click 이벤트를 가장 많이 씀
~~~
v-on:click="balance += parseInt(amount)"
~~~
- v-on 디렉티브는 @로 줄여쓸 수 있음
~~~
@click="balance += parseInt(amount)"
~~~
- Vue 인스턴스에 등록한 메서드를 이벤트 처리 함수로 연결할 수 있음

2. 이벤트 핸들러 메서드
- 복잡한 기능은 메서드를 미리 작성해두고, v-on 디렉티브로 참조해서 이벤트 처리를 수행
~~~
<button id="deposit" v-on:click="deposit" class="btn btn-primary">예금</button>
<button id="withdraw" v-on:click="withdraw" class="btn btn-primary">인출</button>
<script type="text/javascript">
    var vm = new Vue({
        el: "#example",
        data: {
            amount: 0,
            balance: 0
        },
        methods: {
            deposit: function(e) {
                var amt = parseInt(this.amount);
                if (amt < 0) {
                    alert('0원보다 큰 값을 넣어주세요');
                } else {
                    this.balance += amt;
                }
            },
            withdraw: function(e) {
                var amt = parseInt(this.amount);
                if (amt <= 0) {
                    alert('0보다 큰 값을 인출할 수 있습니다');
                } else if (amt > this.balance) {
                    alert('잔고보다 많은 금액을 인출할 수 없습니다.')
                } else {
                    this.balance -= amt;
                }
            },
        }
    })
</script>
~~~
3. 이벤트 객체
- 이벤트를 처리 하는 객체는 첫 번째 파라미터로 이벤트 객체를 전달받음
~~~
deposit: function(e) {~~}
withdraw: function(e) {~~}
~~~
- 이벤트 객체(e)를 통해 이용할 수 있는 정보가 많음
- [이벤트 객체 정보](https://skout90.github.io/2018/01/20/Vue/4.이벤트/)
4. 기본 이벤트
  - HTML 문서나 요소에 어떤 기능을 실행하도록 이미 정의되어 있는 이벤트
    - a 태그 요소를 클릭했을 때, href 특성의 경로로 페이지 이동
    - 브라우저 화면 마우스 오른쪽 클릭하면 내장 컨텍스트 메뉴가 나타남
    - 폼 태그 요소 내부의 submit 버튼을 클릭했을 때, action 특성에 지정된 경로로 method 특성에 지정된 방식으로 전송
    - input 타입 text 요소에 키보드를 누르면 입력한 문자가 텍스트 박스에 나타남
    - 등등..
  - 기본 이벤트의 실행을 막는 방법
    ~~~
    <div id="example" v-on:contextmenu="ctxStop">
      <a href="https://facebook.com" @click="confirmFB">페이스북</a>
    </div>

    methods: {
      ctxStop: function(e) {
          e.preventDefault();
      },
      confirmFB: function(e) {
          if (!confirm('페이스북으로 이동할까요?')) {
              e.preventDefault();
          }
      }
    }
    ~~~
    - 최근에 기본 이벤트의 실행을 막는 주된 이유는 브라우저 화면에서 마우스 오른쪽 버튼을 클릭할 때, 개발자가 직접 작성한 메뉴를 나타내기 위한 경우가 많음
    - 하지만 매번 개발자가 e.preventDefault()를 신경쓰기는 쉽지 않음
    ~~~
      * 아래처럼 작성하면 e.preventDefault()를 안써도되지만, 조건 논리식 결과에 따른 호출일 경우는 써줘야한다.

      <div id="example" v-on:contextmenu.prevent="ctxStop">
      </div>
    ~~~
5. 이벤트 전파와 버블링
  - HTML 문서의 이벤트 처리는 3단계를 거침
    - 1단계 : 이벤트 포착 단계(CAPTURING_PHASE)
      - 문서 내 요소 또는 문서 밖에서부터 이벤트를 발생시킨 HTML 요소까지 포착
    - 2단계 : 이벤트 발생 단계(RAISING_PHASE : AT_TARGET)
      - 요소의 이벤트에 연결된 함수를 직접 호출
    - 3단게 : 버블링(BUBBLING_PHASE) 단계
      - 이벤트가 발생한 요소로부터 상위 요소로 거슬러 올라가면서 동일한 이벤트를 호출
    - 일반적으로 2단계, 3단계에서 연결된 이벤트 함수가 호출
    ~~~
    예제)
    <div id="example">
      <div id="outer" @click="outerClick">
          <div id="inner" @click="innerClick"></div>
      </div>
    </div>

    methods: {
        outerClick: function(e) {
            console.log("### outer click");
            console.log("Event phase : ", e.eventPhase);
            console.log("Current target : ", e.currentTarget);
            console.log("Target : ", e.target);
        },
        innerClick: function(e) {
            console.log("### inner click");
            console.log("Event phase : ", e.eventPhase);
            console.log("Current target : ", e.currentTarget);
            console.log("Target : ", e.target);
        }
    }
    ~~~
    - #inner 클릭하면 #inner, #outer click 이벤트 모두 발생
    - #outer 클릭하면 #outer click 이벤트만 발생
    - BUBBLING_PHASE일 때와 RAISING_PHASE일 때 정보가 다름
    - 즉, currentTarget과 target 값이 서로 다름
      - 버블링 단계에서의 target은 이벤트가 일어난 원본 요소
    - 이벤트 버블링은 막아야한다 (#inner 클릭했을 때 상위 요소로의 이벤트 전파)
      - stopPropagation() 메서드를 호출해서 막을 수 있음
      ~~~
      methods: {
          outerClick: function(e) {
              e.stopPropagation();
          },
          innerClick: function(e) {
              e.stopPropagation();
          }
        }
        })
      ~~~
      - 또는 이벤트 수식어로 대체할 수 있음
        - .stop : 이벤트 전파를 중단
          ~~~
          @click.stop="outerClick"
          @click.stop="innerClick"
          ~~~
        - .capture : CAPTURING_PHASE 단계에서만 이벤트가 발생
          ~~~
          * 위 코드들과 다른 기능
          @click.capture.stop="outerClick"
          @click.stop="innerClick"
          -> #inner 클릭하더라도 CAPTURING_PHASE에서 outerClick이 호출되고 나서 즉시 stop
          즉, 이벤트 포착(capture) 단계에서 전파를 중지
          (outerClick만 호출되고 더 이상 이벤트 발생은 일어나지 않음)
          ~~~
        - .self : RAISING_PHASE 단계일 때만 이벤트가 발생

6. 이벤트 수식어
  - once 수식어 : 한 번만 이벤트를 발생
    ~~~
    v-on:click.once="method"
    ~~~
  - 키코드 수식어 : 키보드 관련 이벤트 처리할 때 사용할 수 있는 수식어
    ~~~
    13(enter)
    v-on:keyup.13="search"

    methods : {
      search :function(e) {
        ~~~
      }
    }
    ~~~
  - 마우스 버튼 수식어 (.left, .right, .middle)
    ~~~
      <div id="example" v-on:contextmenu.prevent="method"
        @mouseup.left="leftMethod" @mouseup.right="rightMethod">
    ~~~

## Part5
### Part5. 스타일
1. 스타일 적용
2. 인라인 스타일
3. CSS 클래스 바인딩
4. 계산형 속성, 메서드를 이용한 스타일 적용
5. 컴포넌트에서의 스타일 적용
6. TodoList 예제

### 6. 컴포넌트 기초
1. 컴포넌트 조합
2. 컴포넌트의 속성
3. DOM 템플릿 구문 작성 시 주의 사항
4. 컴포넌트에서의 data 옵션
5. props와 event
  1. props를 이용한 정보 전달
  2. event를 이용한 정보 전달
  3. props와 event 에제
6. 이벤트 버스 객체를 이용한 통신
7. TodoList 실전 예제

### 7. ECMAScript 2015
1. ES2015를 사용하기 위한 프로젝트 설정
2. let과 const
3. 기본 파라미터와 가변 파라미터
4. 구조분해 할당(destructuring assignment)
5. 화살표 함수(Arrow function)
6. 새로운 객체 리터럴
7. 템플릿 리터럴
8. 컬렉션
9. 클래스
10. 모듈
11. Promise
12. 전개 연산자(Spread Operator)

### 8. Vue-CLI 도구
1. Vue CLI의 구성요소와 설치
2. 프로젝트 생성과 기본 사용법
  1. 프로젝트 생성
  2. 명령어 기본 사용법
  3. vue-cli-service
3. 플러그인
4. vue.config.js
5. Vue CLI GUI 도구

### 9. 컴포넌트 심화
1. 단일 파일 컴포넌트
2. 컴포넌트에서의 스타일
  1. 범위 CSS(Scoped CSS)
  2. CSS 모듈(CSS Module)
3. 슬롯
  1. 슬롯의 기본 사용법
  2. 명명된 슬롯
  3. 범위 슬롯
4. 동적 컴포넌트
5. 재귀 컴포넌트

### 10. axios를 이용한 서버통신
1. 서비스 API 소개
2. axios 기능 테스트
  1. http 프록시 설정
  2. axios 사용
  3. axios 요청 방법
  4. axios 응답 형석
  5. 기타 메서드
  6. 파일 업로드 처리
  7. axios 요청과 config 옵션
  8. Vue 인스턴스에서 axios 이용하기
  9. axios 사용 시 주의 사항
3. 연락처 애플리케이션 예제
  1. 기초 작업
  2. App.vue 작성
  3. ContactList.vue 작성
  4. 입력폼, 수정폼 작성
  5. 사진 변경폼 작성
4. 정리
