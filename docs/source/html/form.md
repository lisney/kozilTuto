기초프로젝트
=============

로그인 폼 document 객체에서 접근 document.frm
--------------------------------------------------

![image](https://user-images.githubusercontent.com/30430227/125771255-759604ee-c8ba-4cd3-bdaf-c64bd11ae718.png)
```
    <section>
        <form action="" name="frm">
            <label for="myId">ID:</label>
            <input type="text" id="myId" name="myId">
            <label for="pwd">password:</label>
            <input type="password" id="pwd" name="pwd">
            <input type="button" name="send" value="로그인">
        </form>
    </section>
    <script>
        const frm = document.frm // form 객체 지정
        let myId = frm.myId
        let pwd = frm.pwd
        const sendBtn = document.frm.send //전송 버튼 객체 지정

        sendBtn.onclick = function(){
            let newId = myId.value,
            newPwd = pwd.value

            if(newId ==''||newPwd==''){
                alert('아이디 또는 비번이 입력되지 않았슴')
            }else{
                alert(myId.value+'님 환영합니다')
            }

        }
    </script>
```
<br>

배열 forms 객체에서 접근 document.forms['frm']
------------------------------------------------
![image](https://user-images.githubusercontent.com/30430227/125779431-856d6014-e0fa-49a1-9225-0f81a509079c.png)
```
    <section>
        <form action="" name="frm">
            <input type="text" name="textA">
            <input type="button" onclick='putText()' value="click">
        </form>
    </section>
    <script>
            function putText(){
                const frm = document.forms['frm']['textA']
                frm.value='값을 입력'
            }

    </script>
```
<br>

this 객체
-------------
'=>' 함수에는 안되더라, =>함수는 항상 '익명'

```
    <ul id="gnb">
        <li>Click</li>
        <li>second</li>
        <li>third</li>
    </ul>
    <script>
        const gnb = document.querySelector('#gnb')
        const li = gnb.querySelectorAll('li')

        li.forEach(l=>{
            l.onclick=function(){
                // l.style.background = 'red'
                this.style.background = 'red'
            }
        })
    </script>
```
<br>

flex, grid 연습
---------------------
![image](https://user-images.githubusercontent.com/30430227/125883950-2a5950a4-a5cb-49b0-b02a-ddb2a68f3aff.png)
```
<body>
    <div class="container">
        <div class="item">1</div>
        <div class="item">2</div>
        <div class="item">3</div>
        <div class="item">4</div>
        <div class="item">5</div>
        <div class="item">6</div>
        <div class="item">7</div>
        <div class="item">8</div>
        <div class="item">9</div>
        <div class="item">10</div>
        <div class="item">+</div>
        <div class="item">-</div>
    </div>
</body>
```
```
     .container{
         /* 그리드 grid */
         width: 400px;
         height: 500px;
         border: 1px solid #000;
         display: grid;
         /* grid-template-rows: 50px repeat(2, 30px) 20px; */
         /* grid-template-columns: repeat(3, 1fr) 50px; */
         grid-template: 50px repeat(2, 60px) 100px /repeat(3, 80px) 50px;
         gap: 20px 10px;
         align-content:center;
         justify-content: space-around;
     }
     .container div{
         /* 플랙스 flex */
         display: flex;
         align-items: center;
         justify-content: center;
         border: 1px solid #000;
         font-size: 2em;
     }
     .item.item:nth-child(1){
         /* row-start/column-start/row-end/column-end */
         grid-area: 1/1/1/5;
     }
     .item:nth-child(3){
         background: orange;
     }
 </style>
```
<br>

문제
-----
![image](https://user-images.githubusercontent.com/30430227/125887779-5a031e89-9f2e-4931-a134-ecdce9af33b0.png)
```
    <form action="" name="frm">
        <p>다음 빈칸에 올바른 값을 입력하시오</p>
        <input type="text" name="a1" size="3" value="9">+
        <input type="text" name="a2" size="3" value="3">=
        <input type="text" name="answer" size="3">
        <br><br>
        <input type="button" onclick="result()" value="확인">
    </form>

    <script>
        function result(){
            const answer = document.frm.answer.value|0
            const correct = document.frm.a1.value*1 + document.frm.a2.value*1

            if(answer == correct){
                window.alert('정답입니다')
            }else{
                window.alert('니주글래')
            }
        }
    </script>
```
<br>

클래스
-------
```
    <script>
        class Character{
        //생성자 함수
            constructor(name, job){
                this.name = name
                this.job = job
            }
            //매서드 함수
            move(){
                document.write(`${this.name}가 ${this.job}을(를) 합니다<br> `)
            }
        }

        const char2 = new Character('플레네', '마법사')
        const char1 = new Character('루이스', '기사')

        char1.move()
        char2.move()

    </script>
```
<br>

클래스 상속
--------------
```
    <script>
        class Character{
            constructor(name, job){
                this.name = name
                this.job = job
            }
            move(){
                document.write(`${this.name}가 ${this.job}을(를) 합니다<br> `)
            }
        }

        class Monster extends Character{
            constructor(name, job, skill){
                //super 는 부모의 생성자를 호출
                super(name, job)
                this.skill = skill
            }
            useSkill(){
                document.write(`${this.name}가 ${this.skill}을 사용합니다 <br>`)
            }
        }

        const char1 = new Monster('오크', '대장','몽둥이')

        char1.move()
        char1.useSkill()
        
    </script>
```
<br>

클로저 함수, 정적 변수(함수 내에서만 사용하는 변수<=>전역 변수)
---------------------------------------------------------------
```
        function outFunc(){
            let staticValue = 0//정적 변수

            function inFunc(val){
                var a = val||1 //매개 변수 val을  a에 대입(값이 없으면 1)
                return staticValue += a
            }
            return inFunc
        }

        var c = outFunc()

        document.write(c()+'<br>')
        document.write(c()+'<br>')
        document.write(c()+'<br>')
    </script>
```
<br>

여러 버튼에 이벤트 
--------------------
![image](https://user-images.githubusercontent.com/30430227/126441797-e3f4361f-ae18-402d-b2bf-3028b436099d.png)
```
    <div class="buttons">
        <button class="btn">button1</button>
        <button class="btn">button2</button>
        <button class="btn">button3</button>
        <button class="btn">button4</button>
    </div>

    <script>
        var btn = document.querySelectorAll('.btn')

        for(let i=0; i<btn.length;++i){ //let 대신 var을 쓰면 안된다
            btn[i].addEventListener('click', function(){
                console.log(i)
            })
        }
    </script>
```
<br>

퀴즈- 수정바람
----------------
![image](https://user-images.githubusercontent.com/30430227/126479864-7ac4bf32-07b4-43c5-9ec6-059816658042.png)
```
<body>
    <div class="grid">
        <h1>퀴즈</h1>
        <div id="quiz">
            <p id="question"></p>
            <div class="buttons">
                <button class="btn"></button>
                <button class="btn"></button>
                <button class="btn"></button>
                <button class="btn"></button>
            </div>

            <footer>
                <p id="progress">진행 정보</p>
            </footer>
        </div>
    </div>
```
```
     .grid{
         width: 600px;
         margin: 50px auto;
         padding: 10px 50px 30px 50px;
         border: 2px solid #000;
         border-radius: 20px;
         box-shadow: 5px 5px 5px black;
     }
     .grid h1{
         color: #333;
         text-align: center;
     }

     .grid #question{
         background: tomato;
         color: white;
         padding: 10px 2em;
         border-radius: 15px;
         text-align: left;
     }
     #quiz{
         text-align: center;
         font-size: 1.2em;
     }

     #progress{
         color: slategray;
     }

     .buttons{
         padding: 30px 20px;
         border: 2px solid #000;
         border-radius: 20px;
     }
     .btn{
         background: lightblue;
         width: 250px;
         font-size: 16px;
         border: 1px solid #000;
         border-radius: 15px;
         margin: 10px 40px 20px 0;
         padding: 10px;
         transition: 0.5s;
     }
     .btn:nth-child(2n){
         margin-right: 0;
     }
     .btn:hover{
         cursor: pointer;
         background: #c34c74;
         color: white;
     }
 </style>
```
```
    <script>
        //문제 객체 생성자
        function Question(text, choice, answer){
            this.text = text
            this.choice = choice
            this.answer = answer
        }

        //퀴즈 정보 객체 생성자
        function Quiz(questions){
            this.score = 0
            this.questions = questions
            this.questionIndex =0
        }

        //Quiz 정답 확인 매서드
        Quiz.prototype.correctAnswer = function(answer){
            return answer == this.questions[this.questionIndex].answer
        }

        //문제 데이터 생성(배열)
        let questions =[
            new Question('다음 중 최초의 상용 웹 바라우저는?',['모자이크','인터넷 익스플로러','구글 크롬','넷스케이프 네비게이터'], '넷스케이프 네이게이터'),
            new Question('웹 문서에서 스타일을 작성하는 언어는?',['HTML','jQuery','CSS','XML'], 'CSS'),
            new Question('명령의 기반의 인터페이스를 의미하는 용어는?',['GUI','CLI','HUD','SI'],'CLI'),
            new Question('명령의 기반의 인터페이스를 의미하는 용어는?',['CLI','GUI','HUD','SI'],'CLI')
        ]

        //퀴즈 인스턴스 객체 생성
        let quiz = new Quiz(questions)

        //문제 출력 함수
        function update_quiz(){
            const question = document.querySelector('#question')
            let idx = quiz.questionIndex + 1
            const choice = document.querySelectorAll('.btn')

            //문제 출력
            question.innerHTML = '문제'+idx+')' + quiz.questions[quiz.questionIndex].text

            //선택 항목 출력
            for(let i =0;i<4;++i){
                choice[i].innerHTML = quiz.questions[quiz.questionIndex].choice[i]
            }

            progress()
        }

        // 문제 진행 정보 표시(현재 문제 번호/총 문항수)
        function progress(){
            var progress = document.querySelector('#progress')
            progress.innerHTML = '문제 ' + (quiz.questionIndex + 1) +' / '+ quiz.questions.length
        }

        // 결과 표시
        function result(){
            var quiz_div = document.querySelector('#quiz')

            //백분율 점수 계산
            var per = parseInt((quiz.score*100)/quiz.questions.length)

            // 표시할 텍스트 정보와 변수
            var txt = '<h1>결과</h1>' +
            '<h2 id="score"> 당신의 점수: ' + quiz.score + '/'+
                quiz.questions.length + '<br><br>' + per + '점</h2>'

            quiz_div.innerHTML = txt

            //점수별 결과 텍스트 출력
            if(per < 60){
                txt +='<h2 style="color:red">더 분발하세요</h2>'
                quiz_div.innerHTML = txt
            }else if(per >=60 && per < 80){
                txt +='<h2 style="color:red">무난한 점수네요</h2>'
                quiz_div.innerHTML = txt
            }else if(per >=80){
                txt += '<h2 style=c"color:red">훌륭합니다</h2>'
                quiz_div.innerHTML = txt
            }
        }

        // -----------------------
        var btn = document.querySelectorAll('.btn')

        //입력 및 정답 확인 함수
        function checkAnswer(i){
            //선택 버튼 이벤트 리스너
            btn[i].addEventListener('click', function(){
                var answer = btn[i].innerText

                if(quiz.correctAnswer(answer)){
                    alert('정답입니다!')
                    quiz.score++
                }else{
                    alert('틀렸습니다')
                }

                // 다음 문제로 진행 및 결과 처리
                if(quiz.questionIndex < quiz.questions.length -1){
                    quiz.questionIndex++
                    update_quiz()
                }else{
                    //결과 화면
                    result()
                }
            })
        }

        // 4개의 버튼 이벤트 리스너 지정
        for(var i = 0; i < btn.length; i++){
            checkAnswer(i)
        }

        update_quiz()

    </script>
```

