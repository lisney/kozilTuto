Git For Window
=====================
Git Bash 한글 안될 때(Bash : 유닉스 쉘)
-------------------------------------------
상단 오른 쪽 클릭 > Options...> Text > Locale :: ko_KR, UTF-8로 변경
<br>

Git for Windows 사용법
----------------------
1. Git Repostory 생성
깃허브 사이트에 들어가서 '+' 버튼을 누르고 생성한다

2.로컬저장소
올릴려는 폴더에서 오른 클릭 후 'Git Bash Here'로 배시를 실행한다.

```
>>깃을 초기화한다(로컬저장소로 만든다)
$ git init

>>올릴 파일이 있는지 확인. (빨간색으로 되어있지만 add작업을 하면 녹색으로 변함)
$ git status

>>올릴 파일 추가
$ git add . (. : 모든 파일, 파일명 혹은 폴더명을 적는다)

>>커밋
$ git commit -m "메시지"

>>깃허브에서 repository 주소 복사, bash에 등록
$ git remote add origin [repository 주소] (ctrl + v 가 안먹히니 마우스 오른클릭)

>> 깃허브와 Bash연결
$ git remote -v

>> 원격  저장소에 올림
$git push origin master
```
```
- git remote ::리모트(원격) 저장소 확인
- git remote ::-v 단축이름과 URL을 함께 봄
- git fetch <remote>  ::리모트 저장소로 가져옴
- git push origin master :: origin은 원격 저장소 이름, master은 현재 사용하는 컴퓨터의 브랜치 이름
```

원격 저장소에서 가져오기
  
`$git pull origin master`

원격 저장소 바꾸기
```
1. 보기 git remote -v
2. 바꾸기 git remote set-url origin 리포지토리 주소
```

error: Your local changes to the following files would be overwritten by merge
발생 시
```
$git stash
$git pull
```

`git clone <원격 주소> `//로컬로 복사해온다( tortose>Git 복제하기)

`tortose 깃>원격>origin:: URL 에 원격주소 붙여넣기`//일반>Git:: 이름, 메일주소 넣기

package.json 을 참조하여 모듈 설치
`git install`

master -> master (fetch first) ...에러발생 시
`git push origin +master로 해결`

할 수 있지만 변경 내용만 push되는 것이 아니라 소스 전체가 다시 push 되는 것으로 보인다.

//기존 데이터를 보장 못할 수도 있기 때문에
```
pull을 받는 다
pull url 을 push url 로 변경한다
pull --force진행한다 //-force, --force?
merge관련 메시지가 발생하는 경우가 있는데 자동화를 위해 이를 무시하려 force옵션을 사용한다.
push
pull url 원복
```
git push 에러창 뜰 때
```
To solve this:
press "i"
write your merge message.
press "esc"
write ":wq"
then press enter.
```
<br>

Mark Down
--------------
```
# 제목 1
## 제목 2
### 제목 3
#### 제목 4
##### 제목 5
###### 제목 6

이텔릭체는 *별표(asterisks)* 혹은 _언더바(underscore)_를 사용하세요.
두껍게는 **별표(asterisks)** 혹은 __언더바(underscore)__를 사용하세요.
**_이텔릭체_와 두껍게**를 같이 사용할 수 있습니다.
취소선은 ~~물결표시(tilde)~~를 사용하세요.
<u>밑줄</u>은 `<u></u>`를 사용하세요.
--- //수평선

1. 순서가 필요한 목록
  - 순서가 필요하지 않은 목록(서브) 
  - 순서가 필요하지 않은 목록(서브) 

[GOOGLE](https://google.com)
[NAVER](https://naver.com "링크 설명(title)을 작성하세요.")
//링크

![대체 텍스트(alternative text)를 입력하세요!](http://www.gstatic.com/webp/gallery/5.jpg "링크 설명(title)을 작성하세요.") //이미지

[![Vue](/images/vue.png)](https://kr.vuejs.org/) //이미지 링크

`background`혹은 `background-image` 속성으로 요소를 강조합니다

###html
<a href="https://www.google.co.kr/" target="_blank">GOOGLE</a>

//블럭 강조
`....`로 감싸기 grave

//테이블

| 값 | 의미 | 기본값 |
|---|:---:|---:|
| `static` | 유형(기준) 없음 / 배치 불가능 | `static` |
| `relative` | 요소 자신을 기준으로 배치 |  |
| `absolute` | 위치 상 부모(조상)요소를 기준으로 배치 |  |
| `fixed` | 브라우저 창을 기준으로 배치 |  |
//헤더 셀을 구분할 때 3개 이상의 -(hyphen/dash) 기호가 필요합니다.
//헤더 셀을 구분하면서 :(Colons) 기호로 셀(열/칸) 안에 내용을 정렬할 수 있습니다.
//가장 좌측과 가장 우측에 있는 |(vertical bar) 기호는 생략 가능합니다.

> 인용문을 작성하세요!
>> 중첩된 인용문(nested blockquote)을 만들 수 있습니다.
>>> 중중첩된 인용문 1
//<blockquote> 태그로 변환됩니다.

//줄 바꾸기 :: #
스페이스 두 번 __

//마크다운 문법이 아닌 원시 HTML 문법을 사용할 수 있습니다.
```



 

