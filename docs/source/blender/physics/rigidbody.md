Rigid Body
===========

<br>

Cell Fracture
------------------

1. 기본

`실행`

![image](https://user-images.githubusercontent.com/30430227/141606394-8d38f041-68e9-4954-bd85-cd42a1c67794.png)

`own vertex - 자신의 점 8개를 기준으로 분리`

![image](https://user-images.githubusercontent.com/30430227/141606460-de8d2916-3fea-498a-bbfb-aef03a73da35.png)
![image](https://user-images.githubusercontent.com/30430227/141606454-75abc78c-5c24-4604-9c74-b8e149775d82.png)

`Child vertex - 자식인 구의 점을 기준으로 갈라짐`

![image](https://user-images.githubusercontent.com/30430227/141606538-b3fead4e-0fff-4f44-93cc-99cc7cd21787.png)
![image](https://user-images.githubusercontent.com/30430227/141606556-b68e8b29-6fe0-4318-b9a3-7c569eaee4dd.png)

`Own Particles - 파티클 기준, Source Limit 이하로만 분리// Child Paricles - 자식의 파티클을 기준으로 갈라짐`

![image](https://user-images.githubusercontent.com/30430227/141606639-bd648dcc-458d-4756-9b73-5618dab32229.png)

![image](https://user-images.githubusercontent.com/30430227/141606648-42db120f-6b73-4f28-85ce-cc325c6c1bcf.png)
![image](https://user-images.githubusercontent.com/30430227/141606658-16ae7365-944b-42f9-8638-6d779e3a6623.png)

![image](https://user-images.githubusercontent.com/30430227/141606671-bdbd107e-17cf-4623-9d15-700fe93b56a9.png)

`Annotation Pencil - Surface`

![image](https://user-images.githubusercontent.com/30430227/141606719-e078cc8d-0dfa-43b5-94ca-d2a679541b96.png)

![image](https://user-images.githubusercontent.com/30430227/141606797-6a279ac1-2c4c-4a75-b5ab-3397c356f1eb.png)
![image](https://user-images.githubusercontent.com/30430227/141606805-79ea7be1-301f-4b2d-b780-f11a0151c9d5.png)

`Multiple - 동시에 적용`

![image](https://user-images.githubusercontent.com/30430227/141606839-f5f48e14-3d09-4e72-b54f-dd0486ac5fcb.png)

![image](https://user-images.githubusercontent.com/30430227/141606948-f9d8de13-4d21-4b7d-a9f6-0f426bda73ed.png)
![image](https://user-images.githubusercontent.com/30430227/141606961-7ff57fb9-1258-4fd9-98f1-670f93141d38.png)

`Noise:: Own, Noise 1 - X, Y, Z 값 영향`

![image](https://user-images.githubusercontent.com/30430227/141607006-9ef977c5-b5fe-4afb-87fa-2f5506b20d4c.png)

`recursion -재귀적, 부분 분할//Source limit 0 : 갈라진데 또 갈라짐//Clamp Recursion: 분할 갯수`

![image](https://user-images.githubusercontent.com/30430227/141607220-5ec8334b-40b9-4c8d-aa03-7c88640008b3.png)

![image](https://user-images.githubusercontent.com/30430227/141607200-8fec4199-5b17-4b7b-ba4e-142559ad67ac.png)

`Material, Recenter, Collection`

![image](https://user-images.githubusercontent.com/30430227/141607483-7e9adcc2-11b5-49ab-a1fb-9bc8ba5eceb8.png)

![image](https://user-images.githubusercontent.com/30430227/141607513-1273ea14-62b4-46f4-a333-4bc1c01c47a6.png)
![image](https://user-images.githubusercontent.com/30430227/141607521-000c040b-0fba-4470-b7d9-be9e4b891068.png)
![image](https://user-images.githubusercontent.com/30430227/141607527-0c4f6839-3578-44d4-b915-f030401ad2f5.png)

`Rigidbody > Add Active`

![image](https://user-images.githubusercontent.com/30430227/141607685-bff558dd-92ed-4645-a698-4881ac0dc2f7.png)

![image](https://user-images.githubusercontent.com/30430227/141607727-ef71a4d1-15f1-4904-9562-db20e5e488a0.png)

> 잠깐! 떨어지는 순간 박스와 조각 컬렉션을 바꿔치기 랜더링 시 Collection 은 랜더링 On/Off 키가 안먹힌다

`Collection > R-mouse > Instance to Scene - 인스턴스 컬렉션은 키프레임이 된다`

![image](https://user-images.githubusercontent.com/30430227/141615506-eb1790df-d94b-4e70-ad5b-4ee64dff7274.png)


밀어내기 - Rigid Body 수동 조작
------------------------------

`Animate 체크 > 포지션 키프레임`

![image](https://user-images.githubusercontent.com/30430227/141653781-2c3def84-8b8b-4f97-a762-8d0c8df2d0af.png)
![image](https://user-images.githubusercontent.com/30430227/141653794-d6272887-f617-4a95-9b04-edd2513e54d3.png)

![image](https://user-images.githubusercontent.com/30430227/141653803-22659d54-8796-4ed6-8dec-61bf6590ad38.png)
![image](https://user-images.githubusercontent.com/30430227/141653815-6dd15737-fd11-426d-86cc-d92dfb052bd7.png)

`부딪히기 전까진 고대로 있기`

![image](https://user-images.githubusercontent.com/30430227/141653827-a6bb2d2c-9bdd-44cd-b34c-a5c752e6a425.png)

<br>

그대로 멈춰라
-------------

```
> Apply Visual Transform - 시믈레이션 된 위치에 고정

센터 구 Passive
Force Field : -500
Gravity: 0
```

![image](https://user-images.githubusercontent.com/30430227/141677995-8fecd0bb-5dd4-49aa-b7a7-061ee3fa926e.png)

![image](https://user-images.githubusercontent.com/30430227/141677961-8cb1b26b-155b-4991-8a23-ca23b15593fc.png)

`작은 구 추가`

![image](https://user-images.githubusercontent.com/30430227/141678036-de5999ae-2be3-454a-ac8b-55eb94074159.png)




