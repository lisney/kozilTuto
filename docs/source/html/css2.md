CSS2
======

기초
-----

1. text-align 은 inline 속성에만 된다. 

2. &nbsp - no-break space

3. 반응형 높이값 - vw사용  height: 30vw;

4. disply:none 적용 안될 때 -  display: none !important;

5. css 파일 변경 

6. inline javascript open window

`<li><a href="#" onclick="window.open('https://www.mediace3d.com/','_blank')">MediACE3D.com</a></li>`

7.  <link rel="icon" type="image/x-icon" href="./images/favicon_mediace3d.png">


```
<link rel="stylesheet" id="changeStyle" href="./style.css">

<a href="javascript:changeStyle()">Can't </a>

...

function changeStyle(){
    document.querySelector("#changeStyle").href="./styleTemp.css"
}
```

<br>

![image](https://user-images.githubusercontent.com/30430227/149711123-244c71df-e787-424f-9c25-01de42b0941e.png)

```
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <h1>나는 고양이를 사랑한다.</h1>

    <ul class="wrapper">
        <li>고양이 먹이 사세요</li>
        <li>운동</li>
        <li>기운내 친구야</li>
    </ul>

    <div class="container">
        <div class="box">부동</div>
        <p>Lorem ipsum dolor sit amet consectetur adipisicing elit. Asperiores excepturi sequi minima tenetur ad, enim laudantium deserunt odio nostrum. Numquam, tempora. Quia, minima. Totam sunt reiciendisexpedita explicabo doloremque perferendis tempore pariatur ex, illo exercitationem non, cupiditate assumenda ea est corrupti voluptate dolores eius neque dolor, quisquam odio nobis fugit sit sapiente. Optio animi saepe voluptates dicta neque dignissimos nulla ipsam, laboriosam, ducimus ipsa enim, reprehenderit temporibus debitis odio quod nisi? Sequi, eveniet sapiente.</p>
    </div>
    <br>

    <div class="position">
        <p>여기가 시작!</p>
        <p class="positioned">여기가 포지션!</p>
        <p>여기가 끝!</p>
    </div>

    <br>

    <form action="">
        <p>우선, 이름과 나이를 말씀해주세요</p>
        <div>
            <label for="이름">이름:</label>
            <input type="text" id="이름">
        </div>
        <div>
            <label for="성">성</label>
            <input type="text" id="성">
        </div>
        <div>
            <label for="나이">나이</label>
            <input type="text" id="나이">
        </div>
        
    </form>
</body>
</html>

```

```
*{
    list-style: none;
    margin: 0;
    padding: 0;
    font-size: 1em;
}

body{
    width: 900px;
    height: 2000px;
    margin: 0 auto;
}

h1{
    font-size: 2rem;
    font-size: 6vw;
    color: red;
}


.wrapper{
    display: flex;
    justify-content: space-around;
}


.wrapper li{
    flex: 1;
}

.wrapper li:first-child{
    display: block;
    flex: 3;
    width: 200px;
    height: 100px;
    background: yellow;
    border: 1px solid #000;
}

/* 컬럼 */

.container{
    margin: 0;
}

.container p{
    /* column-width: 200px; */
    column-count: 2;
}

.box{
    width: 100px;
    height: 100px;
    border: 1px solid #000;
    float: left;
    margin-right: 5px;
}

.position p{
    padding: 10px;
    margin: 10px;
    background: tomato;
    border: 1px solid slateblue;
}

.position .positioned{
    position: relative;
    position: absolute;
    position: fixed;
    position: sticky;
    top: 30px;
    left: 30px;
}
/* r은 현재 위치를 기준 a는 부모의 위치를, fix는 절대위치-스크롤해도 고대로 */
/* s은 스크롤 되다가 top에 도달하면 상단에 붙는다*/

form{
    display: table;
    margin: 0 auto;
}

form p{
    display: table-caption;
    caption-side: bottom;
    width: 300px;
    color: #999;
}

form div{
    display: table-row;
}

form label, form input{
    display: table-cell;
    margin-bottom: 10px;
}

form label{
    width: 200px;
    padding-right: 5%;
    text-align: right;
}

form input{
    width: 300px;
}

/* 반응형 */

@media screen and (min-width: 1000px){
    .container{
        margin: 1em 2em;
    }
    .container p{
        column-count: 3;
        
    }
}

@media (min-width: 700px){
    h1{
        font-size: 4rem;
    }
}

@media (max-width:500px){
    h1{
        font-size: 2em;
    }
}

```


Header
------

1. Sticky Navbar on Scroll

![image](https://user-images.githubusercontent.com/30430227/149715453-289af4af-77e3-45bb-9543-f06e6d42a834.png)

```
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <header>
        <a href="#" class="logo">Logo</a>
        <ul>
            <li><a href="#">Home</a></li>
            <li><a href="#">About</a></li>
            <li><a href="#">Services</a></li>
            <li><a href="#">Portfolio</a></li>
            <li><a href="#">Team</a></li>
            <li><a href="#">Contact</a></li>
        </ul>
    </header>

    <section class="banner"></section>
    
    <script type="text/javascript">
        window.addEventListener("scroll",()=>{
            const header = document.querySelector("header")
            header.classList.toggle("sticky", window.scrollY > 0)
        })
    </script>

</body>
</html>
```

```
@import url('https://fonts.googleapis.com/css2?family=Noto+Sans+KR:wght@100;300;400;500;700;900&display=swap');

*{
    list-style: none;
    text-decoration: none;
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    font-size: 1em;
    font-family: 'Noto Sans KR', sans-serif;
}

body{
    min-height: 200vh;
}

header{
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    display: flex;
    justify-content: space-between;
    align-items: center;
    transition: 0.6s;
    padding: 10px 100px;
    background: rgba(255,255,255,0);
}

/* 자바스크립트 연동 */

header.sticky{
    padding: 5px 100px;
    background: rgba(255,255,255,1) ;
}

header .logo{
    position: relative;
    font-weight: 700;
    color: #000;
    font-size: 2em;
    text-transform: uppercase;
    letter-spacing: 2px;
    transition: 0.6s;
}

header ul{
    position: relative;
    display: flex;
    justify-content: center;
    align-items: center;
}

header ul li{
    position: relative;
}

header ul li a{
    position: relative;
    margin: 0 15px;
    color: #000;
    letter-spacing: 2px;
    font-weight: 500px;
    transition: 0.6s;
}

.banner{
    /* position: relative; */
    width: 100%;
    height: 100vh;
    background: url(images/main1.jpg);
    background-size: contain;
}
.wrapper{
    display: flex;
    justify-content: space-around;
}


.wrapper li{
    flex: 1;
}

.wrapper li:first-child{
    display: block;
    flex: 3;
    width: 200px;
    height: 100px;
    background: yellow;
    border: 1px solid #000;
}

/* 컬럼 */

.container{
    margin: 0;
}

.container p{
    /* column-width: 200px; */
    column-count: 2;
}

.box{
    width: 100px;
    height: 100px;
    border: 1px solid #000;
    float: left;
    margin-right: 5px;
}

.position p{
    padding: 10px;
    margin: 10px;
    background: tomato;
    border: 1px solid slateblue;
}

.position .positioned{
    position: relative;
    position: absolute;
    position: fixed;
    position: sticky;
    top: 30px;
    left: 30px;
}
/* r은 현재 위치를 기준 a는 부모의 위치를, fix는 절대위치-스크롤해도 고대로 */
/* s은 스크롤 되다가 top에 도달하면 상단에 붙는다*/

form{
    display: table;
    margin: 0 auto;
}

form p{
    display: table-caption;
    caption-side: bottom;
    width: 300px;
    color: #999;
}

form div{
    display: table-row;
}

form label, form input{
    display: table-cell;
    margin-bottom: 10px;
}

form label{
    width: 200px;
    padding-right: 5%;
    text-align: right;
}

form input{
    width: 300px;
}

/* 반응형 */

@media screen and (min-width: 1000px){
    .container{
        margin: 1em 2em;
    }
    .container p{
        column-count: 3;
        
    }
}

@media (min-width: 700px){
    h1{
        font-size: 4rem;
    }
}

@media (max-width:500px){
    h1{
        font-size: 2em;
    }
}
```

<br>

Menu
------

1. TAB Table

![image](https://user-images.githubusercontent.com/30430227/149853528-1e32bb86-9bf5-4039-a332-ba4f6ba122b3.png)

```
        <input type="radio" name="tab" id="maf" checked>
        <input type="radio" name="tab" id="stl">
        <nav>
            <label for="maf" class="maf">MAF Files</label>
            <label for="stl" class="stl">STL Files</label>
        </nav>
        <table class="table mafTable">
            <thead>
                <tr>
                    <th>
                        <input type="checkbox" name="maf" id="allMaf" unchecked>
                    </th>
                    <th>No.</th>
                    <th>File</th>
                    <th>File Size</th>
                    <th>Date</th>
                    <th>Expires</th>
                    <th>File Check</th>
                    <th>User</th>
                </tr>
            </thead>
            <tbody>
                <tr>
                    <td><input type="checkbox" name="maf" checked></td>
                    <td>1</td>
                    <td>test.MAF</td>
                    <td>8.5MB</td>
                    <td>2022-01-07 16:02</td>
                    <td>8 days left</td>
                    <td>Valid</td>
                    <td>Donald Trump</td>
                </tr>
```

```

#maf:checked ~ nav label.maf,
#stl:checked ~ nav label.stl{
    color: teal;
    font-weight: 700;
}

section.mylist .table{
    display: none;
}

#maf:checked ~ table.mafTable{
    display: block;
}

#stl:checked ~ table.stlTable{
    display: block;
}

input[type=radio]{
    display: none;
}


section.mylist table{
    width: 100%;
    border-collapse: collapse;
}

section.mylist td, th{
    padding: 12px 15px;
    border: 1px solid #000;
    text-align: center;
    font-size: 1rem;
}

section.mylist th{
    background: teal;
    color: white;
}

section.mylist tbody tr:nth-child(even){
    background:azure;
}
```

<br>

2. 전체 Check Box 선택

![image](https://user-images.githubusercontent.com/30430227/149853226-32e0ffa3-a565-4b80-97aa-12858f37e6c2.png)

```
            <thead>
                <tr>
                    <th>
                        <input type="checkbox" name="stl" id="allStl" unchecked>
                    </th>
                    <th>No.</th>

                </tr>
            </thead>
            <tbody>
                <tr>
                    <td><input type="checkbox" name="stl" unchecked></td>
                    <td>1</td>
               </tr>
```

```
        const allMaf = document.querySelector("#allMaf")

        allMaf.addEventListener('change', ()=>{
            const checkboxes = document.getElementsByName("maf")

            checkboxes.forEach((i)=>{
                i.checked = allMaf.checked
            })
        })
```

<br>

그림자 이미지
------------

![image](https://user-images.githubusercontent.com/30430227/151507338-8aa5b1b6-5b77-4019-9698-c5d1664bfbf0.png)

```
<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <title>Document</title>
  <style>
    *{
      padding: 0;
      margin: 0;
      box-sizing: border-box;
    }
    body{
      width: 100%;
      min-height: 100vh;
      display: flex;
      justify-content: center;
      align-items: center;
    }
    section{
      width: 400px;
      height: 300px;
      display: flex;
      flex-direction: column;
    }
    section .top{
      border: 1px solid rgba(0,0,0,0.3);
      padding: 10px;
      text-align: justify;
    }
    section .btm{
      width: 100%;
      height: 15px;
      display: flex;
      justify-content: space-between;
    }
    .shadowl{
      background: url(./images/best-shadow-left.png) no-repeat;
      background-size: cover;
      width: 200px;
      height: 15px;
    }
    .shadowr{
      background: url(./images/best-shadow-right.png) no-repeat;
      background-size: cover;
      width: 200px;
      height: 15px;
    }
  </style>
</head>
<body>
  <section>
    <div class="top">Lorem ipsum dolor sit amet consectetur adipisicing elit. Distinctio facere pariatur architecto esse modi voluptas sit consectetur, ab a non ullam. Error commodi atque inventore assumenda numquam quod doloremque sequi.</div>
    <div class="btm">
      <div class="shadowl"></div>
      <div class="shadowr"></div>
    </div>
  </section>
</body>
</html>
```

