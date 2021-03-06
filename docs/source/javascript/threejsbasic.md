ThreeJS Basic
===============
<br>
<br>

raycaster 연습
---------------

```
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
    <style>
        #c{
            display: block;
            width: 600px;
            height: 300px;
        }
    </style>
</head>
<body>
    <video src="" width="300" height="300" autoplay muted></video>
    <canvas id="c"></canvas>

    <script type="module">
        const video = document.querySelector('video')
        function startVideo(){
            navigator.mediaDevices.getUserMedia({video})
            .then(stream=>video.srcObject = stream)
            .catch(err=>console.log(err))
        }
        startVideo()

        //캔버스 2D
        const ctx = document.createElement('canvas').getContext('2d')
        ctx.canvas.width = 300
        ctx.canvas.height = 300
        document.body.appendChild(ctx.canvas)

        ctx.fillStyle = 'red'
        ctx.fillRect(0,0,ctx.canvas.width, ctx.canvas.height)

        const img = new Image()
        img.addEventListener('load',()=>{
            ctx.drawImage(img, 10, 10, ctx.canvas.width-20, ctx.canvas.height-20)
        })
        img.src = '../4g2.jpg'

        //three JS
        import * as THREE from './three.module.js'

        const canvas = document.querySelector('#c')
        let renderer, texture, scene, cubes, camera

        function main(){
            renderer = new THREE.WebGLRenderer({canvas})
            scene = new THREE.Scene()
            scene.background = new THREE.Color(0x555555)

            camera = new THREE.PerspectiveCamera(75, 2, 1, 10)
            camera.position.set(0,0,2)
            {
                const light = new THREE.DirectionalLight(0xffffff, 1)
                light.position.set(-1,2,4)
                scene.add(light)
            }

            const geometry = new THREE.BoxGeometry(1,1,1)

            function rand(min, max){
                if(max ===undefined){
                    max = min
                    min = 0
                }
                return min + (max -min)*Math.random()
            }
            texture = new THREE.CanvasTexture(ctx.canvas)
            const textureVideo = new THREE.VideoTexture(video)

            function makeInstance(x){
                const material = [
                        new THREE.MeshPhongMaterial({color:`hsl(${rand(360)|0}, ${rand(50,100)|0}%, 50%)`}),
                        new THREE.MeshPhongMaterial({map: texture}),
                        new THREE.MeshPhongMaterial({color:`hsl(${rand(360)|0}, ${rand(50,100)|0}%, 50%)`}),
                        new THREE.MeshPhongMaterial({map: texture}),
                        new THREE.MeshPhongMaterial({map: textureVideo}),
                        new THREE.MeshPhongMaterial({color:`hsl(${rand(360)|0}, ${rand(50,100)|0}%, 50%)`}),
                    ] 

                const cube = new THREE.Mesh(geometry, material)
                cube.position.set(x,0,0)
                scene.add(cube)

                return cube
            }

            cubes =[
                makeInstance(-2),
                makeInstance(0),
                makeInstance(2),
            ]

            function resizeRendererToDisplaySize(renderer){
                const canvas = renderer.domElement;
                const width = canvas.clientWidth;
                const height = canvas.clientHeight;

                const needResize = width!==canvas.width||height!==canvas.height;

                if(needResize){
                    renderer.setSize(width, height, false)
                }

                return needResize
            }

            function render(time){
                time *= 0.001

                if(resizeRendererToDisplaySize(renderer)){
                    camera.aspect = canvas.width/canvas.height;
                    camera.updateProjectionMatrix()
                }

                cubes.forEach((c,i)=>{
                    const speed = 1 + i*.1
                    const rot = speed *time

                    c.rotation.set(rot,rot,0)
                })
                texture.needsUpdate = true

                renderer.render(scene, camera)

                requestAnimationFrame(render)
                
            }
            requestAnimationFrame(render)

        }

        main()
        
        // const pickPosition = {x:-1, y:-1}
        let pickPosition = new THREE.Vector2()
        const domRect = canvas.getBoundingClientRect()

        function choose(event){
            let Sx = event.clientX -domRect.x
            let Sy = event.clientY -domRect.y
            pickPosition.x = (Sx/canvas.width)*2 -1;
            pickPosition.y = (Sy/canvas.height)*-2 +1

            let raycaster = new THREE.Raycaster()
            raycaster.setFromCamera(pickPosition, camera)
            let intersects = raycaster.intersectObjects(scene.children)
            console.log(domRect.width,intersects)

            if(intersects.length>0){
                intersects[0].object.material[1].transparent = true
                intersects[0].object.material[2].side = THREE.DoubleSide
                intersects[0].object.material[1].opacity = 0.4
            }

        }

        canvas.addEventListener('click', choose)



    </script>
</body>
</html>
```
<br>

이름표를 붙여 1
----------------

```
    <div id="container">
        <canvas id="c"></canvas>
        <div id="labels"></div>
    </div>

>>class-----------------------------------------------
     #container{
         position: relative;
         width: 600px;
         height: 300px;

     }
     #c{
         display: block;
         width: 100%;
         height: 100%;
     }
     #labels{
         position: absolute;
         left: 0;
         top: 0;
         color: white;
     }

     #labels > div{
         position: absolute;
         left: 0;
         top: 0;
         cursor: pointer;
         font-size: xx-large;
         user-select: none;
         text-shadow: 1px 1px 5px black;
     }

     #labels > div:hover{
        color: yellow;
     }

>> script OrbitControls

        import {OrbitControls} from './OrbitControls.js'

            camera = new THREE.PerspectiveCamera(50,2,.1,10)
            camera.position.set(0,0,5)
            scene.add(camera)
            {
                const light = new THREE.PointLight(0xffffff, .3)
                camera.add(light)
            }

            const controls = new OrbitControls(camera,canvas)
            controls.target = scene.position
            controls.update()

>> makeInstance

            const labelContainerElem = document.querySelector('#labels')

            function makeInstance(x, name){
                const material = new THREE.MeshPhongMaterial({color:`hsl(${rand(360)|0}, ${rand(50,100)|0}%, 70%) `})
                // const material = new THREE.MeshPhongMaterial({map:texture})
                const cube = new THREE.Mesh(geometry, material)

                cube.position.set(x,0,0)
                scene.add(cube)

                const elem = document.createElement('div')
                elem.textContent =name;
                labelContainerElem.appendChild(elem)

                return {cube, elem}
            }

            cubes =[
                makeInstance(-2, '4G2'),
                makeInstance(0, 'Kyoko'),
                makeInstance(2,'Dakako')
            ]


>> tempV

            const tempV = new THREE.Vector3()
            

                cubes.forEach((c,i)=>{
                    const {cube, elem} = c;
                    const speed = 1 + i*.1;
                    const rot = speed * time

                    cube.rotation.set(rot, rot, 0)

                    cube.updateWorldMatrix(true, false)
                    cube.getWorldPosition(tempV)
                    //   * 정규화(normalize)된 화면 상의 현재 좌표값을 가져옵니다.
                    // x와 y의 범위는 -1에서 +1까지로, x = -1은 왼쪽, y = -1은 아래쪽입니다.

                    tempV.project(camera)

                    const x = (tempV.x*.5 + .5)*canvas.clientWidth;
                    const y = (tempV.y*-.5 + .5)*canvas.clientHeight;

                    elem.style.transform = `translate(-50%, -50%) translate(${x}px, ${y}px`;
                })
```

<br>

추가
---------

```
const raycaster = new THREE.Raycaster()


> raycaster.setFromCamera(tempV, camera)
> const intersects = raycaster.intersectObjects(scene.children)
const show = intersects.length >0 && cube===intersects[0].object
if(!show||Math.abs(tempV.z)>1){
        elem.style.display ='none'
    }else{
        elem.style.display =''
    }

```
<br>

Cube 백 개 무작위로
-------------------

```
const boxWidth = 1;
const boxHeight = 1;
const boxDepth = 1;
const geometry = new THREE.BoxGeometry(boxWidth, boxHeight, boxDepth);
 
function rand(min, max) {
  if (max === undefined) {
    max = min;
    min = 0;
  }
  return min + (max - min) * Math.random();
}
 
function randomColor() {
  return `hsl(${ rand(360) | 0 }, ${ rand(50, 100) | 0 }%, 50%)`;
}
 
const numObjects = 100;
for (let i = 0; i < numObjects; ++i) {
  const material = new THREE.MeshPhongMaterial({
    color: randomColor(),
  });
 
  const cube = new THREE.Mesh(geometry, material);
  scene.add(cube);
 
  cube.position.set(rand(-20, 20), rand(-20, 20), rand(-20, 20));
  cube.rotation.set(rand(Math.PI), rand(Math.PI), 0);
  cube.scale.set(rand(3, 6), rand(3, 6), rand(3, 6));
}
```
<br>

# Rig Nodes
--------------

```
> Input > input Armature
> Add Input , name 'create_rig'
> Flow Control > Execute > InputArmature - Execute , SetPreview(본이 나타난다)
> 연필아이콘 Edit mode > 위 쪽 본 끝을 선택하고 Select to Cursor, Y축 방향으로 이동
> 본을 복사한다, 하나는 실린더에 하나는 뚜껑 힌지 부위에 이동
> 루프노드 추가 Loop

> Loop 추가 후 'TAB' > Bone>SetBoneProperty 추가
> Edit->Pose로 변경 > AddInput 버튼, Rotation Mode 추가, XYZ Euler
> GetBones - Get(LoopIndex -Index) - Bone
> Loop 나오기 > SetPreview버튼

> Loop의 End Index값을 자동으로
> GetBone - Length - EndIndex

> Hirearchy 수정
> Bone>SetBoneProperty 추가 > Add Input, Parent 선택 > 본 Edit로 들어가 이름을 바꾼다음
> Parent 지정

> SetPreview, Edit모드->Pose모드에서 테스트해본다

> Circle 추가 > x 축으로 이동한 후, 복사 4개 점을 선택한 후 SelectInverse, dissolve->스퀘어 생성, 스퀘어복사 넓게

> SetBoneProperty 추가, PoseMode, CustomObject 선택
> AddInput::CustomShapeScale 추가

> 바인딩
> 뚜껑 선택 본 선택 Shift + tab 뚜껑쉐이프 선택 + Ctrl + P(Bone)
```
