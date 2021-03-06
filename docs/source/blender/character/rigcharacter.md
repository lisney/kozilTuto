리깅 캐릭터
=========

웨이트
----------

1. 웨이트 리셋

![image](https://user-images.githubusercontent.com/30430227/124885153-f08e8200-e00d-11eb-861b-42ac6af28546.png)  
![image](https://user-images.githubusercontent.com/30430227/124885360-26336b00-e00e-11eb-9c13-b136bd293870.png)  
선택 영역(메쉬, 본)의 웨이트를 Vertex Groups에서 Remove한다

<br>
2. 벨트 웨이트

![image](https://user-images.githubusercontent.com/30430227/124885796-98a44b00-e00e-11eb-9efa-5625f4073dd2.png)  
힙 본과 척추 본

![image](https://user-images.githubusercontent.com/30430227/124885699-7e6a6d00-e00e-11eb-9e67-e3bfaf9bd540.png)  
힙 본은 전체에 웨이트 100% Asign

![image](https://user-images.githubusercontent.com/30430227/124885947-c2f60880-e00e-11eb-9249-e0a17b1032e7.png)  
척추 본은 일단 100% 준 후 아랫부분만 25% 다시 준다

<br>

3. 허벅지 웨이트

![image](https://user-images.githubusercontent.com/30430227/124886762-870f7300-e00f-11eb-8203-4da4117224a5.png)  
오른쪽 허벅지 본에 적용(왼쪽도 마찬가지)

![image](https://user-images.githubusercontent.com/30430227/124886997-baea9880-e00f-11eb-8b16-6043e850e03e.png)  
이 부분은 힙 본에도 적용

<br>

4. 머리 웨이트

![image](https://user-images.githubusercontent.com/30430227/124887289-fd13da00-e00f-11eb-8aa5-e4fe068fd089.png)  
![image](https://user-images.githubusercontent.com/30430227/124887339-0ac95f80-e010-11eb-99b2-111281bfc1a3.png)  
목 본에는 Remove, 머리 본에는 100%

<br>

5. 머리, 팔 본

Inherit Rotation

<br>

손가락

![image](https://user-images.githubusercontent.com/30430227/124888545-27b26280-e011-11eb-8aa3-1eb252785c13.png)  
![image](https://user-images.githubusercontent.com/30430227/124888878-77912980-e011-11eb-9981-6b041e1d6dbe.png)  
Copy Rotation Constraint  

 <br>
 
![image](https://user-images.githubusercontent.com/30430227/124889183-bfb04c00-e011-11eb-8d83-e9c6068af17a.png)  
중지 본도 Constraint 적용(Influence: 20정도)  

<br>

![image](https://user-images.githubusercontent.com/30430227/124889802-64cb2480-e012-11eb-9482-01e05f7a7732.png)  
X축 정렬(Roll: Ctrl + r)  

<br>

![image](https://user-images.githubusercontent.com/30430227/124890378-f3d83c80-e012-11eb-8f00-50dd5114e8d0.png)  
![image](https://user-images.githubusercontent.com/30430227/124890556-1d916380-e013-11eb-8710-f87e452efe48.png)  
Constraint, x축만 적용   

<br>

![image](https://user-images.githubusercontent.com/30430227/124891945-60a00680-e014-11eb-8c75-2fa518ef755a.png)  
![image](https://user-images.githubusercontent.com/30430227/124891983-6b5a9b80-e014-11eb-9587-012dbde170e6.png)  
두 번째 마디는 Limit Rotation Constraint도 적용(세 번째 마디를 회전한 상태에서 한계값을 정한다)  

<br>

![image](https://user-images.githubusercontent.com/30430227/124892652-005d9480-e015-11eb-81a8-0bbad25c1b94.png)  
![image](https://user-images.githubusercontent.com/30430227/124892697-0b182980-e015-11eb-809e-933acb0fb1b4.png)  

<br>

오른 손 본만 제거하기 위해서 X-Axis Mirror 체크 해지 다음 제거
<br>
![image](https://user-images.githubusercontent.com/30430227/124893164-7661fb80-e015-11eb-8e87-9d87a6c6757b.png)  
왼 손 본 오른 손 본에 복사  
  <br>
![image](https://user-images.githubusercontent.com/30430227/124895109-369c1380-e017-11eb-91bf-107e679b5b12.png)  
발 본 복사(Clear Parent), name: foot.IK.L, IK 적용  
  <br>
![image](https://user-images.githubusercontent.com/30430227/124895717-c17d0e00-e017-11eb-810e-acaf9218b5d6.png)  
발 본에 Copy Rotation Constraint 적용
  <br>
![image](https://user-images.githubusercontent.com/30430227/124897121-f50c6800-e018-11eb-9642-c277a22d457c.png)  
회전용 본 생성  
  <br>
![image](https://user-images.githubusercontent.com/30430227/124897279-1ec58f00-e019-11eb-92fb-c6e9bcd256d9.png)  
foot.IK.L 본이 roll.F.L을 아버지라 부르게 됨  
  <br>
![image](https://user-images.githubusercontent.com/30430227/124897645-7663fa80-e019-11eb-86bb-811cc8ad28a8.png)  
roll.B.L이 할아버지가 됨  
  <br>
![image](https://user-images.githubusercontent.com/30430227/124899128-c7c0b980-e01a-11eb-9449-931f9a3d8308.png)  
foot.IK.main.L 이 증조부  
  <br>
![image](https://user-images.githubusercontent.com/30430227/125006847-03e72f00-e09a-11eb-8809-dd384320d886.png)  
![image](https://user-images.githubusercontent.com/30430227/125006687-ac48c380-e099-11eb-8a88-1f04cee95999.png)  
Constraint Local  

  <br>
  
![image](https://user-images.githubusercontent.com/30430227/125006865-0c3f6a00-e09a-11eb-9d6e-5ff6ce6d7421.png)  
![image](https://user-images.githubusercontent.com/30430227/124918260-ce0d6080-e02f-11eb-813a-3c9ec2e2ae10.png)  
Invert

 <br>
 
![image](https://user-images.githubusercontent.com/30430227/125006873-11041e00-e09a-11eb-8aee-749d0be0e7ec.png)

![image](https://user-images.githubusercontent.com/30430227/124919468-46285600-e031-11eb-9b59-e656fa2e5f2d.png)

Invert

 PoleTarget
![image](https://user-images.githubusercontent.com/30430227/125007092-7a842c80-e09a-11eb-844c-6d4b43656eca.png)
![image](https://user-images.githubusercontent.com/30430227/125007118-8839b200-e09a-11eb-827a-34ce28e9e4af.png)

<br>

6. Copy, SwitchDir, Parent

![image](https://user-images.githubusercontent.com/30430227/125007436-26c61300-e09b-11eb-832f-65b6a1cc8edc.png)

![image](https://user-images.githubusercontent.com/30430227/125007501-48bf9580-e09b-11eb-9eed-d23b9e5e905d.png)


