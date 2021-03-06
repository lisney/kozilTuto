LPLevel Rigging
---------------

**커스텀 프로퍼티 - 회전값 Expression**

``Copy New Driver > Paste Driver(Rotate) > to Radian(radians(prop))`` 

![image](https://user-images.githubusercontent.com/30430227/161925933-d7cbf67c-0a12-449b-95ae-298750baa68c.png)
![image](https://user-images.githubusercontent.com/30430227/161926179-6717b206-60de-412e-b2f5-5b7f1af2b283.png)

**또는 Python 사용 회전**

``bpy.context.scene.frame_current/10``

![image](https://user-images.githubusercontent.com/30430227/162128564-cb397160-0cae-4283-9758-707285d972c5.png)

**If Expression**

.. code-block:: console 

 0 if switch == 1 or switch == 3 else 1
 switch 가 1 또는 3이면 0 그렇지 않으면 1
 
![image](https://user-images.githubusercontent.com/30430227/162124508-c6f53a30-e953-4c6c-9b30-78dc573cc521.png)
![image](https://user-images.githubusercontent.com/30430227/162124324-193e9849-a6f4-4228-9190-e3cb55a77b7c.png)


Sci-Fi Door
------------

![image](https://user-images.githubusercontent.com/30430227/162122820-8eca7cf7-51e9-444f-a91b-594712058e3a.png)
![image](https://user-images.githubusercontent.com/30430227/162122927-bb807f9b-0dc9-442d-b578-1b224cb8cb60.png)

``사라지는 문 Lattice > Shape Key``

![image](https://user-images.githubusercontent.com/30430227/162110119-48d13a4f-059d-40a7-bf82-9a251054b112.png)
![image](https://user-images.githubusercontent.com/30430227/162110201-09b5d205-0ffd-4ad7-b1a9-7b772d5860be.png)

``Apply Modifier > Move Door``

![image](https://user-images.githubusercontent.com/30430227/162110305-3790d235-ac7c-4040-8373-858e75b97b28.png)
![image](https://user-images.githubusercontent.com/30430227/162110331-cf2c51d8-6d8a-4893-92d1-e2761f71afb1.png)

``Root bone > Custom Property> Door bone Y-Location(Driven)``

![image](https://user-images.githubusercontent.com/30430227/162117579-ad2ad113-06d0-4182-b442-96bfa9aca303.png)
![image](https://user-images.githubusercontent.com/30430227/162117635-43e564ac-10df-44c8-ad11-0364f57bdb04.png)

![image](https://user-images.githubusercontent.com/30430227/162117651-57667de3-3dd9-4822-826c-56358c7b04d7.png)
![image](https://user-images.githubusercontent.com/30430227/162117994-2c9679a9-9207-4086-b2cd-cea2665816dd.png)

**Door 2**

``시간 차 오픈 top,bottom door Z-Axis > Limits``

![image](https://user-images.githubusercontent.com/30430227/162120234-0b63497e-161f-4ea5-a8ac-d9f6c7ed1a06.png)

![image](https://user-images.githubusercontent.com/30430227/162120669-e533e9cc-dece-42ae-98b9-b45d8e56a8fe.png)

![image](https://user-images.githubusercontent.com/30430227/162121468-9759067a-2b09-4da9-892f-54ee692b5990.png)
![image](https://user-images.githubusercontent.com/30430227/162121537-0f8c9ed3-590a-4594-82e5-cbcbb5f6c80e.png)

![image](https://user-images.githubusercontent.com/30430227/162121703-94450114-a3ee-4662-9472-d1f94da0fa8c.png)
![image](https://user-images.githubusercontent.com/30430227/162121575-16c49428-87fc-4874-9b5c-c9fdf7c5cdc1.png)
![image](https://user-images.githubusercontent.com/30430227/162121676-f8b5f5cd-27e8-45b3-a210-8d7f25de9104.png)

**Shader Driven(Fac)**

![image](https://user-images.githubusercontent.com/30430227/162122688-d99d9b58-adbe-4ab7-978a-804343bebcfc.png)
![image](https://user-images.githubusercontent.com/30430227/162122702-020993fc-b15b-4ae7-91d4-029301a507aa.png)

![image](https://user-images.githubusercontent.com/30430227/162122655-f21a7710-65fe-4700-86d2-75159a09f408.png)

**Multi Piston**

![image](https://user-images.githubusercontent.com/30430227/162127861-6278b54a-11eb-4f05-ba84-770518d6bec3.png)
![image](https://user-images.githubusercontent.com/30430227/162127896-732480b5-4351-422b-836b-7cba08c185f5.png)


``Root parent > Damped Track > Parent > Copy Transforms``
 
![image](https://user-images.githubusercontent.com/30430227/162126459-ae8d6100-1ae0-4f63-98f8-02e244c6cb86.png)
![image](https://user-images.githubusercontent.com/30430227/162126833-e942e6df-8a29-44b6-9f80-295c0cff3609.png)

![image](https://user-images.githubusercontent.com/30430227/162127010-73f2a175-ed70-4de3-bcbb-249e6cccd58b.png)
![image](https://user-images.githubusercontent.com/30430227/162127064-34615542-2652-4c5a-878d-fa2fd5c9f3cc.png)

![image](https://user-images.githubusercontent.com/30430227/162127776-0f59dee5-be73-4609-80c5-dc0e4bc0406e.png)
![image](https://user-images.githubusercontent.com/30430227/162127820-d8e96374-895b-42f0-b3a7-fac3d9846ba4.png)

**Eye Rig**

``Shape Keys > New Shape from Mix > UP, Down Vertex Group``

![image](https://user-images.githubusercontent.com/30430227/162130749-6cae8b83-8297-42f2-81bd-a63294bc0f3e.png)
![image](https://user-images.githubusercontent.com/30430227/162130968-24341306-3f78-4ac8-8b57-14c7afd52824.png)

![image](https://user-images.githubusercontent.com/30430227/162131228-a5e66046-beee-4025-b336-fa128f4b0ce6.png)

![image](https://user-images.githubusercontent.com/30430227/162131357-23fb2a5e-8bd7-4f2d-b048-d0d5620425a3.png)

``Shape Key Driver``

![image](https://user-images.githubusercontent.com/30430227/162131964-e5b5d387-e310-4348-b5da-101b6dbfc716.png)
![image](https://user-images.githubusercontent.com/30430227/162131854-6a80143a-3e6d-45ba-a38a-0522c6d8b532.png)

![image](https://user-images.githubusercontent.com/30430227/162131824-3139d7b0-d5ff-4ce7-aeeb-24d92ed957f5.png)


2D Rigging
============

GP Stroke Rig
--------------

![image](https://user-images.githubusercontent.com/30430227/162156967-d8732522-5cad-4f3e-b9af-25aa4e69b84d.png)
![image](https://user-images.githubusercontent.com/30430227/162157109-a4769a6f-6e70-40cc-9e33-b21074536e6a.png)

![image](https://user-images.githubusercontent.com/30430227/162157306-99cefd75-6375-4e40-a8dc-3399e56beb30.png)
![image](https://user-images.githubusercontent.com/30430227/162157184-bdc9d0a3-0ffb-4c82-8177-b4dc6fc13f5d.png)

``빨간색 Constraint >CopyLocation(머리녹색본)>StrecthTo(꼬리녹색본)``

![image](https://user-images.githubusercontent.com/30430227/162158178-9e775f03-bd98-45fb-92bc-c7120d681b01.png)

![image](https://user-images.githubusercontent.com/30430227/162157393-9befd063-9a14-4b12-8960-da99db92054c.png)


GP Arm Rig
-----------

``Lattice > Vertex Groups(옷만)``

![image](https://user-images.githubusercontent.com/30430227/162162036-8258b52c-856b-49d8-a21c-f5c0b177a326.png)
![image](https://user-images.githubusercontent.com/30430227/162165904-c43e6849-24d7-4747-a016-4bfc3ed012ef.png)

![image](https://user-images.githubusercontent.com/30430227/162166003-2f6135bc-9873-4861-9319-94225b8a1794.png)

``Bone > Lattice 수동 Weight > 손 >Layer Relations``

![image](https://user-images.githubusercontent.com/30430227/162167480-24b97bb4-fcba-457b-aaf9-b823ee77ed88.png)
![image](https://user-images.githubusercontent.com/30430227/162167518-322168a6-25d2-4921-abd3-48c1d092e32f.png)

![image](https://user-images.githubusercontent.com/30430227/162167805-b89891ff-be73-448e-998f-30930ae5a33f.png)

``Bendy Bone Volume 문제 > ``

![image](https://user-images.githubusercontent.com/30430227/162168173-276f4cfc-c985-4904-bf7b-31f0949bb072.png)
![image](https://user-images.githubusercontent.com/30430227/162168453-d644d647-0388-4d81-bf49-5f55780ccd2f.png)

![image](https://user-images.githubusercontent.com/30430227/162168420-24c4f934-0569-481a-935d-22ce243481d7.png)

GP Eye Rig
-------------

![image](https://user-images.githubusercontent.com/30430227/162350899-141908dc-8f0c-4b47-bdb4-c1eec3e6f5cb.png)

``Draw(Layer 'eye')> Edit Mode >Shift-S(keep offset) >Move Left``

![image](https://user-images.githubusercontent.com/30430227/162345028-932e3e76-6256-4155-981f-9fe07a778913.png)
![image](https://user-images.githubusercontent.com/30430227/162345078-3642a457-df7b-45a1-bc37-2cd8d7d4ba2f.png)
![image](https://user-images.githubusercontent.com/30430227/162345311-2effc0b1-d5e8-416a-b666-1cf76c9efaba.png)

``Duplicate Layer 'pupil'> Scale > Mask``

![image](https://user-images.githubusercontent.com/30430227/162345775-e6c6ed29-78c0-4067-9548-df35662fc9b0.png)g
![image](https://user-images.githubusercontent.com/30430227/162345525-4fc5fd92-d677-449e-9a33-b70c2e04aedb.png)
![image](https://user-images.githubusercontent.com/30430227/162345840-07c1ae0d-f8e3-4d3a-af35-4361441fe6e3.png)

``Mirror Modifier > Apply Modifier(Object Mode)``

![image](https://user-images.githubusercontent.com/30430227/162346024-2a031bfc-e4fe-42b1-89d5-2ab62120e197.png)
![image](https://user-images.githubusercontent.com/30430227/162346046-98636fae-4232-438d-8d9b-96e7636aee0c.png)

``Add Bone>Custom Shape``

![image](https://user-images.githubusercontent.com/30430227/162349202-1a145241-0e06-4470-a869-20b9048d36d2.png)
![image](https://user-images.githubusercontent.com/30430227/162349422-38760980-112d-4567-b77f-ccf245c6d0bf.png)

``Empty Group > Assign``

![image](https://user-images.githubusercontent.com/30430227/162349503-9f41e304-9a72-4707-a502-283153b09635.png)

![image](https://user-images.githubusercontent.com/30430227/162349790-355c84f2-fc0e-4fa6-a365-1a155f8fec4a.png)
![image](https://user-images.githubusercontent.com/30430227/162349830-acb43379-8a1f-4c8c-b83b-12b75df28e95.png)

``Custom Shape 시 Wireframe 체크하면 좋다는데?``

![image](https://user-images.githubusercontent.com/30430227/162350855-18a2c508-d0d4-4e2d-a369-925355391ffc.png)


