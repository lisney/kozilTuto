TypeScript Tutorial
========================
<br>
<br>

멀티뷰
-------

![image](https://user-images.githubusercontent.com/30430227/122009753-5adc4a00-cdf5-11eb-998e-c36d4f7c4f60.png)

```
<!DOCTYPE html>
<html lang="ko">
<head>
 <meta charset="UTF-8">
 <meta name="viewport" content="width=device-width, initial-scale=1.0">
 <meta http-equiv="X-UA-Compatible" content="ie=edge">
 <title>title</title>
 <style>
     *{
         box-sizing: border-box;
         margin: 0;
     }
     .c{
         position: relative;
         /* display: block; */
         width: 300px;
         height: 300px;
     }
 </style>
</head>
<body>
<canvas id="c1" class="c"></canvas>
<canvas id="c2" class="c"></canvas>
<canvas id="c3" class="c"></canvas>
<canvas id="c4" class="c"></canvas>

<script type="module">
    import * as THREE from './three.module.js'
    import {OrbitControls} from './OrbitControls.js'

    const canvas1 = document.querySelector('#c1')
    const canvas2 = document.querySelector('#c2')
    const canvas3 = document.querySelector('#c3')
    const canvas4 = document.querySelector('#c4')

    let renderer1, renderer2, renderer3, renderer4, camera1, camera2, camera3, camera4, controls, scene, cube

    init()
    animate()

    function init(){
        renderer1 = new THREE.WebGLRenderer({canvas:canvas1})
        renderer2 = new THREE.WebGLRenderer({canvas:canvas2})
        renderer3 = new THREE.WebGLRenderer({canvas:canvas3})
        renderer4 = new THREE.WebGLRenderer({canvas:canvas4})

        camera1 = new THREE.PerspectiveCamera(75,1,.1,10)
        camera2 = new THREE.OrthographicCamera(-1,1,1,-1,.1,10)
        camera3 = new THREE.OrthographicCamera(-1,1,1,-1,.1,10)
        camera4 = new THREE.OrthographicCamera(-1,1,1,-1,.1,10)
        
        camera1.position.z = 2
        camera2.position.y = 1
        camera2.lookAt(new THREE.Vector3(0,0,0))
        camera3.position.z =1
        //layers 카메라와 대상 모델은 같은 레이어를 공유한다. 기본 레이어:0, 
        //layers.enable():지정한 레이어도 공유, layers.set():지정레이어만 공유
        camera3.layers.set(1)
        camera4.position.x = 1
        camera4.lookAt(new THREE.Vector3(0,0,0))
        camera4.layers.set(1)

        scene = new THREE.Scene()
        scene.add(camera2)

        const gridHelper1 = new THREE.GridHelper(5,10)
        const gridHelper2 = new THREE.GridHelper(5,10)
        const gridHelper3 = new THREE.GridHelper(5,10)
        gridHelper2.layers.set(1)
        gridHelper2.rotation.set(Math.PI/2,0,0)
        gridHelper3.layers.set(1)
        gridHelper3.rotation.set(0,0,Math.PI/2)
        scene.add(gridHelper1)
        scene.add(gridHelper2)
        scene.add(gridHelper3)


        controls = new OrbitControls(camera1, renderer1.domElement)

        const geometry = new THREE.BoxGeometry()
        const material = new THREE.MeshBasicMaterial({color:0x00ff00, wireframe:true})

        cube = new THREE.Mesh(geometry, material)
        scene.add(cube)
        cube.layers.enable(1)
    }

    function animate(time){
        time *=0.001
        cube.rotation.set(time, time,0)
        controls.update()

        renderer1.setSize(canvas1.width, canvas1.height, false)

        renderer1.render(scene, camera1)
        renderer2.render(scene, camera2)
        renderer3.render(scene, camera3)
        renderer4.render(scene, camera4)

        requestAnimationFrame(animate)
    }


</script>
</body>
</html>
 ```
 <br>
<br>

 GUI
 -----
 
 ![image](https://user-images.githubusercontent.com/30430227/119974723-f8f7a400-bfef-11eb-8f42-32a64a58757f.png)

 
 ```
     <canvas id="c"></canvas>
    <script type="module">
        import * as THREE from './three.module.js'
        import {OrbitControls} from './OrbitControls.js'
        import Stats from './stats.module.js'
        import {GUI} from './dat.gui.module.js'


        const canvas1 = document.querySelector('#c')

        const renderer = new THREE.WebGLRenderer()
        renderer.setSize(window.innerWidth/window.innerHeight)
        document.body.appendChild(renderer.domElement)

        const scene = new THREE.Scene()
        const camera = new THREE.PerspectiveCamera(75,2,.1,10)
        camera.position.set(0,0,2)
        
        const controls = new OrbitControls(camera, renderer.domElement)
        controls.addEventListener('change', render)

        const geometry = new THREE.BoxGeometry()
        const material = new THREE.MeshBasicMaterial({color:0x00ff00, wireframe:true})
        const cube = new THREE.Mesh(geometry, material)
        scene.add(cube)

        window.addEventListener('resize', onWindowResize, false)
        function onWindowResize(){
            camera.aspect = window.innerWidth/window.innerHeight
            camera.updateProjectionMatrix()
            renderer.setSize(window.innerWidth, window.innerHeight)
        }
        onWindowResize()

        const stats = Stats()
        // stats.showPanel(2)
        document.body.appendChild(stats.dom)

        const gui = new GUI()
        const cubeFolder = gui.addFolder('cube')
        cubeFolder.add(cube.rotation, 'x', 0,Math.PI*2, 0.01)
        cubeFolder.add(cube.rotation, 'y', 0,Math.PI*2, 0.01)
        cubeFolder.add(cube.rotation, 'z', 0,Math.PI*2, 0.01)
        cubeFolder.open()
        const cameraFolder = gui.addFolder('camera')
        cameraFolder.add(camera.position, 'z', 0,10,0.01)
        cameraFolder.open()
        
        function render(){
            stats.begin()
            renderer.render(scene, camera)
            stats.end()
            requestAnimationFrame(render)
        }
        render()
 ```
 <br>
<br>

 GUI 추가
 ------------
 
 ```
     <script type="module">
        import * as THREE from './js/three.module.js'
        import {GUI} from './js/dat.gui.module.js'

        const api ={
            dataString:'Brush',
            dataNumber: 0.7,
            dataBoolean:false,
            dataFunction: function(){
                alert(
                    'dataString:'+this.dataString+'\n'+
                    'dataNumber'+ this.dataNumger+'\n'
                )
            },
            dataColor: 0xffff00, //대신 [255,255,255]를 하면 컬러 Text가 바뀐다

        }

        window.onload = function(){
            const gui = new GUI()

            gui.add(api, 'dataString')
            gui.add(api, 'dataString',['Brush','Lisney','Kyoko']) //입력받을 수 있는 문자열을 제한
            gui.add(api, 'dataString').options(['Brush','Lisney','Kyoko']) //위와 같다
            
            gui.add(api,'dataNumber')
            gui.add(api,'dataNumber', 0,10,1)
            gui.add(api,'dataNumber', {A:0, B:10, C:20})  //수치 타입을 항목으로써 선택할려면
            gui.add(api, 'dataBoolean')

            const myData = gui.addFolder('내문서')
            myData.add(api, 'dataFunction')
            myData.addColor(api, 'dataColor')
            myData.open()
            
            const renderer = new THREE.WebGLRenderer()
            renderer.setSize(300,300)
            document.body.appendChild(renderer.domElement)
            
            const scene = new THREE.Scene()
            scene.background = new THREE.Color('#00ff00')

            myData.addColor(new ColorGUIHelper(scene,'background'),'value').onFinishChange(
                v=>{
                    renderer.render(scene, camera) //씬은 다시 랜더링해줘야 색상이 update된다
                }
                )
                
                const camera = new THREE.PerspectiveCamera(75,2,.1,10)
                
                renderer.render(scene, camera)
            
        }
	//컬러 클래스없이 바로 배경색을 바꿀 수 있는 방식
	        gui.addColor(api,'dataColor').onFinishChange(v=>{
            scene.background = new THREE.Color(v)
            renderer.render(scene, camera)
        })
	//컬러 클래스없이 바로 배경색을 바꿀...

        class ColorGUIHelper{
            constructor(object, prop){
                this.object = object
                this.prop = prop
            }
            get value(){
                return `#${this.object[this.prop].getHexString()}`
            }
            set value(hexString){
                this.object[this.prop].set(hexString)
            }
        }



    </script>

그런데 만약, 값의 변경이 dat.gui가 아닌 다른 곳에서 이루어진 다면, listen을 호출해 주면 됩니다.
gui.add(myData, "dataNumber").listen();

dat.gui를 통한 값의 변경이 발생하면,
gui.add(myData, "dataNumber").onChange(
     v=>{
     ...
     })
     
onChange는 값 변경 중의 매 순간 발생하, onFinishChange는 최종적인 값의 변경이 발생할 때 호출되는 이벤트

```
 <br>
<br>
Drag Control
---------------

```
    <script type="module">
        import * as THREE from './js/three.module.js'
        import {DragControls} from './js/DragControls.js'
        import Stats from './js/stats.module.js'

        const canvas = document.querySelector('#c')

        function main(){
            const renderer = new THREE.WebGLRenderer({canvas})

            const scene = new THREE.Scene()
            const axesHelper = new THREE.AxesHelper(5)
            scene.add(axesHelper)
            
            const gridHelper = new THREE.GridHelper(5,20,'red', 'dodgerblue') //size, numbers, axisColor, color
            gridHelper.rotation.set(Math.PI/2,0,0)
            scene.add(gridHelper)

            const camera = new THREE.PerspectiveCamera(50, 2, 0.1, 10)
            camera.position.set(0,0,5)

            {
                const light = new THREE.PointLight()
                light.position.set(10,10,10)
                scene.add(light)
            }

            const geometry = new THREE.BoxGeometry(1,1,1)

            function rand(min,max){
                if(max===undefined){
                    max = min;
                    min =0;
                }
                return min + (max - min)*Math.random()
            }

            function makeInstance(x){
                const material = new THREE.MeshPhongMaterial({
                    color:`hsl(${rand(360)|0}, ${rand(50,100)|0}%, 90%)`,
                    // color:'white',
                    transparent: true,
                })
                const cube = new THREE.Mesh(geometry, material)
                cube.position.set(x,0,0)

                scene.add(cube)

                return cube
            }

            const cubes =[
                makeInstance(-2),
                makeInstance(0),
                makeInstance(2),
            ]

            const controls = new DragControls(cubes, camera, renderer.domElement)
            
            controls.addEventListener('dragstart',event=>{
                event.object.material.opacity =0.33
            })
            controls.addEventListener('dragend',event=>{
                event.object.material.opacity =1
            })

            const stats = new Stats()
            document.body.appendChild(stats.dom)


            function render(){

                renderer.render(scene, camera)
                requestAnimationFrame(render)
            }
            requestAnimationFrame(render)

        }

        main()
```
 <br>
<br>
transform/multiple Control
------------------------------
```

        import {TransformControls} from './js/TransformControls.js'

            const tControls = new TransformControls(camera, renderer.domElement)
            tControls.setSize(3)
            tControls.space = 'local' //기본은 world

            tControls.attach(cubes[1])
            scene.add(tControls)
            // click 이벤트와 choose 함수를 사용하여 물체를 선택하게할 수 있을듯
            // 사용가능 event : mousedown, mouseup 등
            
                        window.addEventListener('keydown',event=>{
                switch(event.key){
                    case 'w':
                        tControls.setMode('translate')
                        break
                    case 'e':
                        tControls.setMode('rotate')
                        break
                    case 'r':
                        tControls.setMode('scale')
                        break
                }
            })
            
```
 <br>
<br>
Raycaster face normal
-----------------------

![image](https://user-images.githubusercontent.com/30430227/123203478-69abb680-d4f1-11eb-8a65-b05009bfe7b2.png)

```
<!DOCTYPE html>
<html lang="ko">
<head>
 <meta charset="UTF-8">
 <meta name="viewport" content="width=device-width, initial-scale=1.0">
 <meta http-equiv="X-UA-Compatible" content="ie=edge">
 <title>title</title>
 <style>
     #c{
         display: block;
         width: 600px;
         height: 300px;
     }
 </style>
</head>
<body>
<canvas id="c"></canvas>

<script type="module">
    import * as THREE from './three.module.js'
    import {OrbitControls} from './OrbitControls.js'
    import {GLTFLoader} from './GLTFLoader.js'
    import Stats from './stats.module.js'

    let renderer, scene, arrowHelper, camera, controls, stats
    let raycaster, domRect, sceneMeshes, mouse
    let coneGeometry, material

    const canvas = document.querySelector('#c')

    init()
    animate()

    function init(){
        renderer = new THREE.WebGLRenderer({canvas})
        renderer.physicallyCorrectLights = true
        renderer.shadowMap.enabled = true
        renderer.outputEncoding = THREE.sRGBEncoding

        scene = new THREE.Scene()
        scene.background = new THREE.Color('aqua')
        const gridHelper = new THREE.GridHelper(10,10,'blue','gray')
        const axesHelper = new THREE.AxesHelper(5)
        arrowHelper = new THREE.ArrowHelper(new THREE.Vector3(), new THREE.Vector3(),.5, 'yellow')
        scene.add(gridHelper)
        scene.add(axesHelper)
        scene.add(arrowHelper)


        camera = new THREE.PerspectiveCamera(75, 2, .1, 20)
        camera.position.set(0,2,4)
        controls = new OrbitControls(camera, canvas)

        sceneMeshes = new Array()

        const loader = new GLTFLoader()
        loader.load('../gltfs/raycaster.gltf', gltf=>{
            gltf.scene.traverse(child=>{
                if(child.isMesh){
                    let m = child
                    m.castShadow = true
                    m.receiveShadow = true
                    // m.material.flatShading = true
                    sceneMeshes.push(m)
                }
                if(child.isLight){
                    let l = child
                    l.castShadow = true
                    l.shadow.bias = -.001
                    l.shadow.mapSize.width = 2048
                    l.shadow.mapSize.height = 2048
                    l.intensity = 2
                }
            })
            scene.add(gltf.scene)
        },xhr=>console.log(xhr.loaded/xhr.total*100 +'% 로딩'),
        error=>console.log(error))

        coneGeometry = new THREE.ConeGeometry(.2,.8,8)
        material = new THREE.MeshNormalMaterial()

        raycaster = new THREE.Raycaster()
        domRect = canvas.getBoundingClientRect()
        mouse = new THREE.Vector2()

        stats = Stats()
        document.body.appendChild(stats.dom)
    }

    function resizeRendererToDisplaySize(renderer){
        const canvas = renderer.domElement
        const width = canvas.clientWidth
        const height = canvas.clientHeight

        const needResize = width!==canvas.width||height!==canvas.height

        if(needResize){
            renderer.setSize(width,height, false)
        }

        return needResize
    }

    function animate(){
        if(resizeRendererToDisplaySize(renderer)){
            camera.aspect = canvas.clientWidth/canvas.clientHeight
            camera.updateProjectionMatrix()
        }
        controls.update()
        stats.update()
        renderer.render(scene, camera)
        requestAnimationFrame(animate)
    }

    canvas.addEventListener('mousemove', onMouseMove, false)
    canvas.addEventListener('dblclick', onDoubleClick, false)

    function onMouseMove(event){
        mouse.x = (event.clientX - domRect.x)/canvas.clientWidth *2 -1
        mouse.y = (event.clientY -domRect.y)/canvas.clientHeight *-2 +1

        raycaster.setFromCamera(mouse, camera)
        const intersects = raycaster.intersectObjects(sceneMeshes, false)
        if(intersects.length>0){
            let n = new THREE.Vector3()
            n.copy(intersects[0].face.normal)
            // n.transformDirection(intersects[0].object.matrixWorld)
            arrowHelper.position.copy(intersects[0].point)
            arrowHelper.setDirection(n)
        }
    }

    function onDoubleClick(event){
        raycaster.setFromCamera(mouse, camera)
        const intersects = raycaster.intersectObjects(sceneMeshes, false)
        if(intersects.length>0){
            let n = new THREE.Vector3()
            n.copy(intersects[0].face.normal)
            n.transformDirection(intersects[0].object.matrixWorld)
            const cone = new THREE.Mesh(coneGeometry, material)
            cone.lookAt(n)
            cone.rotateX(Math.PI/2)
            cone.position.copy(intersects[0].point)
            cone.position.addScaledVector(n,.2)
            scene.add(cone)
            sceneMeshes.push(cone)

        }
    }
</script>
</body>
</html>
```
 <br>
<br>
drag(boxHelper)
----------------

![image](https://user-images.githubusercontent.com/30430227/120190946-3f4d3d00-c254-11eb-96f1-6c42a549fc4a.png)
```
    <script type="module">
        import * as THREE from './three.module.js'
        import {OrbitControls} from './OrbitControls.js'
        import {DragControls} from './DragControls.js'

        import Stats from './stats.module.js'
        import {GLTFLoader} from './GLTFLoader.js'

        const canvas = document.querySelector('#c')

        function main(){
            const renderer = new THREE.WebGLRenderer({canvas})
            renderer.shadowMap.enabled = true

            const scene = new THREE.Scene()
            
            const axesHelper = new THREE.AxesHelper(5)
            scene.add(axesHelper)

            const camera = new THREE.PerspectiveCamera(75, 2, 0.1, 10)
            camera.position.set(0,0,4)

            const orbitControls = new OrbitControls(camera, canvas)
            orbitControls.target.set(0,0,0)

            {
                const light1 = new THREE.PointLight(0xffffff, .9)
                light1.position.set(2.5,2.5,2.5)
                light1.castShadow = true
                scene.add(light1)

                const light2 = new THREE.PointLight(0xffffff, .5)
                light2.position.set(-2.5,2.5,2.5)
                light2.castShadow = true
                scene.add(light2)
            }

            const sceneMeshes = new Array()
            let boxHelper

            const dragControls = new DragControls(sceneMeshes, camera, canvas)
            dragControls.addEventListener('hoveron', event=>{
                boxHelper.visible = true
                orbitControls.enabled = false
            })
            dragControls.addEventListener('hoveroff',event=>{
                boxHelper.visible = false
                orbitControls.enabled= true
            })
            dragControls.addEventListener('drag',event=>{
                event.object.position.y = 0
            })
            dragControls.addEventListener('dragstart', event=>{
                boxHelper.visible = true
                orbitControls.enabled = false
            })
            dragControls.addEventListener('dragend',event=>{
                boxHelper.visible = false
                orbitControls.enabled = true
            })

            const planeGeometry = new THREE.PlaneGeometry(25,25)
            const texture = new THREE.TextureLoader().load('../4g2.jpg')
            const plane = new THREE.Mesh(planeGeometry, new THREE.MeshPhongMaterial({map:texture}))
            plane.rotateX(-Math.PI/2)
            plane.position.y = -1
            plane.receiveShadow = true
            scene.add(plane)

            let mixer
            let modelReady = false
            const gltfLoader = new GLTFLoader()
            let modelGroup
            let modelDragBox

            gltfLoader.load('../gltfs/pyramid.gltf', gltf=>{
                gltf.scene.traverse(child=>{
                    if(child){
                        modelGroup = child
                    }
                    if(child.isMesh){
                        child.castShadow = true
                        child.geometry.computeVertexNormals()
                    }

                    modelDragBox = new THREE.Mesh(new THREE.BoxGeometry(2,2,2), new THREE.MeshBasicMaterial({transparent: true, opacity:0}))
                    modelDragBox.geometry.translate(0,0,0) //// center the geometry
                    scene.add(modelDragBox)
                    sceneMeshes.push(modelDragBox)

                    boxHelper = new THREE.BoxHelper(modelDragBox, 0xffff00)
                    boxHelper.visible = false
                    scene.add(boxHelper)

                    scene.add(gltf.scene)

                    modelReady = true
                },xhr=>{
                    console.log((xhr.loaded/xhr.total *100) + '% loaded')//로딩되고 있는 동안 실행
                },error=>{
                    console.log(error)
                })
            })

            function resizeRendererToDisplaySize(renderer){
                const canvas = renderer.domElement
                const width = canvas.clientWidth
                const height = canvas.clientHeight

                const needResize = width!==canvas.width||height!==canvas.height

                if(needResize){
                    renderer.setSize(width,height, false)
                }
                return needResize
            }

            const stats = Stats()
            document.body.appendChild(stats.dom)

            const clock = new THREE.Clock()

            function render(){
                if(resizeRendererToDisplaySize(renderer)){
                    camera.aspect= canvas.width/canvas.height  //window.innerWidth / window.innerHeight
                    camera.updateProjectionMatrix()
                }

                stats.update()
                orbitControls.update()

                if(modelReady){
                    modelGroup.position.copy(modelDragBox.position)
                    boxHelper.update()
                }

                renderer.render(scene, camera)
                requestAnimationFrame(render)
                
            }
            requestAnimationFrame(render)

        }

        main()
```
 <br>
<br>
mouse Pick
---------------

![image](https://user-images.githubusercontent.com/30430227/120286203-f3a09f00-c2f8-11eb-909d-d58f43247327.png)
```
    <script type="module">
        import * as THREE from './three.module.js'
        import {OrbitControls} from './OrbitControls.js'
        import {DragControls} from './DragControls.js'

        import Stats from './stats.module.js'
        import {GLTFLoader} from './GLTFLoader.js'

        const canvas = document.querySelector('#c')

        function main(){
            const renderer = new THREE.WebGLRenderer({canvas})
            renderer.shadowMap.enabled = true
            renderer.outputEncoding = THREE.sRGBEncoding

            const scene = new THREE.Scene()
            {
                const light1 = new THREE.PointLight(0xffffff, 0.7)
                const light2 = new THREE.PointLight(0xffffff, 0.7)
                light1.position.set(2,2,2)
                light2.position.set(-2,2,2)
                light1.castShadow = true
                light2.castShadow = true
                scene.add(light1)
                scene.add(light2)
            }

            const camera = new THREE.PerspectiveCamera(75, 2, .1, 100)
            camera.position.set(5,5,5)

            const controls = new OrbitControls(camera, renderer.domElement)

            const pickObjects = new Array()
            let intersectedObject
            let originalMaterials = {}
            const hightlightedMaterial = new THREE.MeshBasicMaterial({wireframe:true, color:0x00ff00})

            const loader = new GLTFLoader()
            loader.load('../gltfs/items.gltf', gltf=>{
                gltf.scene.traverse(child=>{
                    if(child.isMesh){
                        let m = child
                        switch (m.name){
                            case 'Plane':
                                m.receiveShadow = true
                                break
                            case 'Sphere':
                                m.castShadow = true
                                break
                            default:
                                m.castShadow = true
                                pickObjects.push(m)
                                originalMaterials[m.name] = m.material
                        }
                    }
                })
                scene.add(gltf.scene)
            },xhr=>{
                console.log((xhr.loaded/xhr.total * 100)+'% loaded')
            },error=>{
                console.log(error)
            })

            const raycaster = new THREE.Raycaster()
            let intersects

            document.addEventListener('mousemove', onDocumentMouseMove, false)
            const domRect = canvas.getBoundingClientRect()

            function onDocumentMouseMove(event){
                raycaster.setFromCamera({
                    x: ((event.clientX -domRect.x)/renderer.domElement.clientWidth)*2 -1,
                    y: ((event.clientY - domRect.y)/renderer.domElement.clientHeight)*-2+1
                }, camera)
                intersects = raycaster.intersectObjects(pickObjects, false)

                if(intersects.length>0){
                    intersectedObject = intersects[0].object
                }else{
                    intersectedObject = null
                }
                pickObjects.forEach((o, i)=>{
                    if(intersectedObject && intersectedObject.name == o.name){
                        pickObjects[i].material = hightlightedMaterial
                    }else{
                        pickObjects[i].material = originalMaterials[o.name]
                    }
                })
            }

            function resizeRendererToDisplaySize(renderer){
                const canvas = renderer.domElement
                const width = canvas.clientWidth
                const height = canvas.clientHeight

                const needResize = width!==canvas.width||height!==canvas.height

                if(needResize){
                    renderer.setSize(width,height, false)
                }
                return needResize
            }

            const stats = Stats()
            document.body.appendChild(stats.dom)

            const clock = new THREE.Clock()

            function render(){
                if(resizeRendererToDisplaySize(renderer)){
                    camera.aspect= canvas.width/canvas.height  //window.innerWidth / window.innerHeight
                    camera.updateProjectionMatrix()
                }

                stats.update()
                controls.update()

                renderer.render(scene, camera)
                requestAnimationFrame(render)
                
            }
            requestAnimationFrame(render)

        }

        main()
```
 <br>
<br>
measure
-------------

![image](https://user-images.githubusercontent.com/30430227/121800689-4ae62e00-cc6e-11eb-8e20-74c5cb0bbbcd.png)

```
<!DOCTYPE html>
<html lang="ko">
<head>
 <meta charset="UTF-8">
 <meta name="viewport" content="width=device-width, initial-scale=1.0">
 <meta http-equiv="X-UA-Compatible" content="ie=edge">
 <title>title</title>
 <style>
     #c{
         display: block;
         width: 600px;
         height: 300px;
     }
     #instruction{
         color: white;
         background: #000;
         position: absolute;
         left: 50%;
         top: 15px;
         margin-left: -120px;
     }
     .measurementLabel{
         position: absolute;
         background: #000;
         color: white;
     }
 </style>
</head>
<body>
<canvas id="c"></canvas>
<div id="instruction">Ctrl + 마우스 Left 버튼을 눌러 선을 그리시오</div>
<script type="module">
    import * as THREE from './three.module.js'
    import {OrbitControls} from './OrbitControls.js'
    import {GLTFLoader} from './GLTFLoader.js'
    import Stats from './stats.module.js'
    import {CSS2DRenderer, CSS2DObject} from './CSS2DRenderer.js'

    const canvas = document.querySelector('#c')

    let renderer, scene, camera, controls, stat
    let labelRenderer

    init()
    animate()

    function init(){
        renderer = new THREE.WebGLRenderer({canvas})
        renderer.physicallyCorrectLights = true
        renderer.outputEncoding = THREE.sRGBEncoding
        renderer.shadowMap.enabled = true

        scene = new THREE.Scene()
        scene.background = new THREE.Color('teal')

        camera = new THREE.PerspectiveCamera(50, 2, .1,30)
        camera.position.set(0, 10, 10)
        controls = new OrbitControls(camera, canvas)
        controls.enableDamping = true
        {
            const light = new THREE.SpotLight(0xffffff, 10)
            light.position.set(15,15,15)
            light.castShadow = true
            light.shadow.bias = -.001
            light.shadow.mapSize.width = 2048
            light.shadow.mapSize.height = 2048
            scene.add(light)
        }

        const sceneObjects = new Array()

        const loader = new GLTFLoader()
        loader.load('../gltfs/simple.gltf',gltf=>{
            gltf.scene.traverse(child=>{
                if(child.isMesh){
                    let m = child
                    switch(m.name){
                        case 'Plane':
                            m.receiveShadow = true
                            break
                        default:
                            m.castShadow = true
                    }
                    sceneObjects.push(m)
                }
            })
            scene.add(gltf.scene)
        },xhr=>console.log(xhr.loaded/xhr.total*100 + '% loaded'),
        error=>console.log(error)
        )

        labelRenderer = new CSS2DRenderer()
        labelRenderer.setSize(canvas.clientWidth, canvas.clientHeight)
        labelRenderer.domElement.style.position ='absolute'
        labelRenderer.domElement.style.top = '0px'
        labelRenderer.domElement.style.pointerEvents ='none'
        document.body.appendChild(labelRenderer.domElement)

        let ctrlDown = false
        let lineId = 0
        let line
        let drawingLine = false
        const measurementLabels ={}

        window.addEventListener('keydown',event=>{
            if(event.key==='Control'){
                ctrlDown = true
                controls.enabled = false
                renderer.domElement.style.cursor = 'crosshair'
            }
        })

        window.addEventListener('keyup',event=>{
            if(event.key==='Control'){
                ctrlDown = false
                controls.enabled = true
                renderer.domElement.style.cursor = 'pointer'
                if(drawingLine){
                    //완료되지 않은 라인은 삭제
                    scene.remove(line)
                    scene.remove(measurementLabels[lineId])
                    drawingLine = false
                }
            }
        })

        const raycaster = new THREE.Raycaster()
        let intersects
        const mouse = new THREE.Vector2()

        renderer.domElement.addEventListener('pointerdown',onClick, false)

        function onClick(event){
            if(ctrlDown){
                raycaster.setFromCamera(mouse, camera)
                intersects = raycaster.intersectObjects(sceneObjects, false)
                if(intersects.length>0){
                    if(!drawingLine){
                        //라인 그리기 시작
                        const points =[]
                        points.push(intersects[0].point)
                        points.push(intersects[0].point.clone())
                        const geometry = new THREE.BufferGeometry().setFromPoints(points)
                        line = new THREE.LineSegments(geometry, new THREE.LineBasicMaterial({
                            color:0xff0000, transparent:true, opacity:.75
                        }))
                        line.frustumCulled = false
                        scene.add(line)
    
                        const measurementDiv = document.createElement('div')
                        measurementDiv.className = 'measurementLabel'
                        measurementDiv.innerText ='0.0m'
    
                        const measurementLabel = new CSS2DObject(measurementDiv)
                        measurementLabel.position.copy(intersects[0].point)
                        measurementLabels[lineId] = measurementLabel
                        scene.add(measurementLabels[lineId])
                        drawingLine = true
                    }else{
                        //라인 그리기 마침
                        const positions = line.geometry.attributes.position.array
                        positions[3] = intersects[0].point.x
                        positions[4] = intersects[0].point.y
                        positions[5] = intersects[0].point.z
                        line.geometry.attributes.position.needsUpdate = true
                        lineId++
                        drawingLine = false
                    }
                }
            }
        }
        document.addEventListener('mousemove', onMouseMove, false)

        const domRect = canvas.getBoundingClientRect()

        function onMouseMove(event){
            //기본 브라우저? 이벤트 금지
            event.preventDefault()
            mouse.x =((event.clientX-domRect.y)/canvas.clientWidth)*2 -1
            mouse.y = ((event.clientY-domRect.y)/canvas.clientHeight)*-2 +1

            if(drawingLine){
                //두 번째 점의 위치에 따라 선 및 라벨의 위치 변경
                raycaster.setFromCamera(mouse, camera)
                intersects = raycaster.intersectObjects(sceneObjects, false)
                if(intersects.length>0){
                    const positions = line.geometry.attributes.position.array
                    const v0 = new THREE.Vector3(positions[0], positions[1], positions[2])
                    const v1 = new THREE.Vector3(intersects[0].point.x, intersects[0].point.y, intersects[0].point.z)
                    positions[3] = intersects[0].point.x
                    positions[4] = intersects[0].point.y
                    positions[5] = intersects[0].point.z
                    line.geometry.attributes.position.needsUpdate = true
                    const distance = v0.distanceTo(v1)
                    measurementLabels[lineId].element.innerText = distance.toFixed(2) +'m'
                    measurementLabels[lineId].position.lerpVectors(v0, v1, .5)
                }
            }
        }
    }


    function resizeRendererToDisplaySize(renderer){
        const canvas = renderer.domElement
        const width = canvas.clientWidth
        const height = canvas.clientHeight

        const needResize = width!==canvas.width||height!==canvas.height

        if(needResize){
            renderer.setSize(width, height, false)
        }

        return needResize
    }
    function animate(){
        if(resizeRendererToDisplaySize(renderer)){
            camera.aspect = canvas.clientWidth/canvas.clientHeight
            camera.updateProjectionMatrix()
        }
        controls.update()
        renderer.render(scene, camera)
        labelRenderer.render(scene, camera)

        requestAnimationFrame(animate)
    }
    

</script>
</body>
</html>
```
 <br>
<br>
블렌더에서 GLTF 보내기
-------------------------

```
> 라이트 Include:Puncual Lights 체크

> Bone Aniamtion
 각각 Track 클립 넣기 >  클립 교차되지 않게 위치 옮기기
 이름 바꾸기 : Animation Data 에서 애니메이션을 선택한 후 이름바꾼다
 시작 프레임 위치로 옮긴 후 Export

```
![image](https://user-images.githubusercontent.com/30430227/120104397-3c3c4900-c18f-11eb-8143-51211c5121ec.png)
[그램] 트랙에 배치한 후 mute 한다(체크해제)


# gltf animation
![image](https://user-images.githubusercontent.com/30430227/120741003-3cda3400-c52f-11eb-8f96-e6df8bb82058.png)

```
			import * as THREE from './three.module.js';

			import Stats from './stats.module.js';
			import { GUI } from './dat.gui.module.js';

			import { GLTFLoader } from './GLTFLoader.js';

			let container, stats, clock, gui, mixer, actions, activeAction, previousAction;
			let camera, scene, renderer, model, face;

			const api = { state: 'actionA' };

			const canvas = document.querySelector('#c')

			init();
			animate();

			function init() {
				renderer = new THREE.WebGLRenderer( {canvas:canvas, antialias: true } );
				renderer.outputEncoding = THREE.sRGBEncoding;
	
				camera = new THREE.PerspectiveCamera( 45, 2, 0.25, 100 );
				camera.position.set( -3, 3, 7 );
				camera.lookAt( new THREE.Vector3( 0, 2, 0 ) );

				scene = new THREE.Scene();
				scene.background = new THREE.Color( 0xe0e0e0 );
				scene.fog = new THREE.Fog( 0xe0e0e0, 20, 100 );

				clock = new THREE.Clock();

				// lights

				const hemiLight = new THREE.HemisphereLight( 0xffffff, 0x444444 );
				hemiLight.position.set( 0, 20, 0 );
				scene.add( hemiLight );

				const dirLight = new THREE.DirectionalLight( 0xffffff );
				dirLight.position.set( 0, 20, 10 );
				scene.add( dirLight );

				// ground

				const mesh = new THREE.Mesh( new THREE.PlaneGeometry( 2000, 2000 ), new THREE.MeshPhongMaterial( { color: 0x999999, depthWrite: false } ) );
				mesh.rotation.x = - Math.PI / 2;
				scene.add( mesh );

				const grid = new THREE.GridHelper( 200, 40, 0x000000, 0x000000 );
				grid.material.opacity = 0.2;
				grid.material.transparent = true;
				scene.add( grid );

				// model

				const loader = new GLTFLoader();
				loader.load( '../gltfs/actions.gltf', function ( gltf ) {

					model = gltf.scene;
					scene.add( model );

					createGUI( model, gltf.animations );

				}, undefined, function ( e ) {

					console.error( e );

				} );


				// stats
				stats = new Stats();
				document.body.appendChild( stats.dom );

			}

			function createGUI( model, animations ) {

				const states = [ 'actionA', 'actionB' ];
				const emotes = [ 'Jump', 'Yes', 'No', 'Wave', 'Punch', 'ThumbsUp' ];

				gui = new GUI();

				mixer = new THREE.AnimationMixer( model );

				actions = {};

				for ( let i = 0; i < animations.length; i ++ ) {

					const clip = animations[ i ];
					const action = mixer.clipAction( clip );
					actions[ clip.name ] = action;

					if ( emotes.indexOf( clip.name ) >= 0 || states.indexOf( clip.name ) >= 4 ) {

						action.clampWhenFinished = true;
						action.loop = THREE.LoopOnce;

					}

				}

				// states

				const statesFolder = gui.addFolder( 'States' );

				const clipCtrl = statesFolder.add( api, 'state' ).options( states );

				clipCtrl.onChange( function () {

					fadeToAction( api.state, 0.5 );

				} );

				statesFolder.open();

				// emotes

				// const emoteFolder = gui.addFolder( 'Emotes' );

				// function createEmoteCallback( name ) {

				// 	api[ name ] = function () {

				// 		fadeToAction( name, 0.2 );

				// 		mixer.addEventListener( 'finished', restoreState );

				// 	};

				// 	emoteFolder.add( api, name );

				// }

				// function restoreState() {

				// 	mixer.removeEventListener( 'finished', restoreState );

				// 	fadeToAction( api.state, 0.2 );

				// }

				// for ( let i = 0; i < emotes.length; i ++ ) {

				// 	createEmoteCallback( emotes[ i ] );

				// }

				// emoteFolder.open();

				// expressions

				face = model.getObjectByName( 'CubeA' );

				const expressions = Object.keys( face.morphTargetDictionary );
				const expressionFolder = gui.addFolder( 'Expressions' );

				for ( let i = 0; i < expressions.length; i ++ ) {

					expressionFolder.add( face.morphTargetInfluences, i, 0, 1, 0.01 ).name( expressions[ i ] );

				}

				activeAction = actions[ 'actionA' ];
				activeAction.play();

				expressionFolder.open();

			}

			function fadeToAction( name, duration ) {

				previousAction = activeAction;
				activeAction = actions[ name ];

				if ( previousAction !== activeAction ) {

					previousAction.fadeOut( duration );

				}

				activeAction
					.reset()
					.setEffectiveTimeScale( 1 )
					.setEffectiveWeight( 1 )
					.fadeIn( duration )
					.play();

			}

			//

            function resizeRendererToDisplaySize(renderer){
                const canvas = renderer.domElement
                const width = canvas.clientWidth
                const height = canvas.clientHeight

                const needResize = width!==canvas.width||height!==canvas.height

                if(needResize){
                    renderer.setSize(width, height, false)
                }

                return needResize
            }

			function animate() {

				const dt = clock.getDelta();

				if ( mixer ) mixer.update( dt );

                if(resizeRendererToDisplaySize(renderer)){
                    camera.aspect = canvas.width/canvas.height
                    camera.updateProjectionMatrix()
                }
                
				renderer.render( scene, camera );
				requestAnimationFrame( animate );

				stats.update();

			}


```
 <br>
<br>
Multi Controls
------------------

![image](https://user-images.githubusercontent.com/30430227/121470318-fdad5680-c9f8-11eb-828e-bd057815ae14.png)

```
<!DOCTYPE html>
<html lang="ko">
<head>
 <meta charset="UTF-8">
 <meta name="viewport" content="width=device-width, initial-scale=1.0">
 <meta http-equiv="X-UA-Compatible" content="ie=edge">
 <title>title</title>
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

    startVideo()
    function startVideo(){
        navigator.mediaDevices.getUserMedia({video})
        .then(stream=>video.srcObject = stream)
        .catch(err=>console.log(err))
    }

    const ctx = document.createElement('canvas').getContext('2d')
    ctx.canvas.width = 300
    ctx.canvas.height = 300
    document.body.appendChild(ctx.canvas)

    ctx.fillStyle = 'red'
    ctx.fillRect(0,0,300,300)

    const img = new Image()
    img.addEventListener('load',()=>{
        ctx.drawImage(img, 20,20,260,260)
    })
    img.src = '../4g2.jpg'

    
    import * as THREE from './three.module.js'
    import {OrbitControls} from './OrbitControls.js'
    import {DragControls} from './DragControls.js'
    import {TransformControls} from './TransformControls.js'
    import Stats from './stats.module.js'
    
    let renderer, scene, camera, cubes, stats
    const canvas = document.querySelector('#c')
    
    main()
    animate()
    
    function main(){
        
        renderer = new THREE.WebGLRenderer({canvas})
        scene = new THREE.Scene()
        const axesHelper = new THREE.AxesHelper(5)
        scene.add(axesHelper)
        
        camera = new THREE.PerspectiveCamera(75, 2, 0.1, 10)
        camera.position.set(0,0,2)

        const geometry = new THREE.BoxGeometry()
        const material = new THREE.MeshNormalMaterial({transparent:true})

        const cube = new THREE.Mesh(geometry, material)
        scene.add(cube)

        const orbitControls = new OrbitControls(camera, canvas)

        const dragControls = new DragControls([cube], camera, canvas)
        dragControls.addEventListener('hoveron',()=>{
            orbitControls.enabled = false
        })
        dragControls.addEventListener('hoveroff',()=>{
            orbitControls.enabled = true
        })
        dragControls.addEventListener('dragstart',(event)=>{
            event.object.material.opacity = 0.33
        })
        dragControls.addEventListener('dragend',(event)=>{
            event.object.material.opacity = 1
        })
        stats = Stats()
        document.body.appendChild(stats.dom)

        const texture = new THREE.TextureLoader().load('../umbrellas.png',()=>{
            const rt = new THREE.WebGLCubeRenderTarget(texture.image.height)
            rt.fromEquirectangularTexture(renderer, texture)
            scene.background = rt.texture
        })

        const transformControls = new TransformControls(camera, canvas)
        transformControls.attach(cube)
        transformControls.setMode('rotate')
        transformControls.size =1
        scene.add(transformControls)

        transformControls.addEventListener('dragging-changed',(event)=>{
            orbitControls.enabled = !event.value
            dragControls.enabled = !event.value
        })

        window.addEventListener('keydown', (event)=>{
            switch(event.key){
                case 'w':
                    transformControls.setMode('translate')
                    break

                case 'e':
                    transformControls.setMode('rotate')
                    break

                case 'r':
                    transformControls.setMode('scale')
                    break
            }
        })
        
    }

    
    function animate(){
        if(resizeRendererToDisplaySize(renderer)){
            camera.aspect = canvas.width/canvas.height
            camera.updateProjectionMatrix()
        }
        stats.update()
        renderer.render(scene, camera)

        requestAnimationFrame(animate)
    }

    function resizeRendererToDisplaySize(renderer){
        const canvas = renderer.domElement
        const width = canvas.clientWidth
        const height = canvas.clientHeight

        const needResize = width!==canvas.width||height!==canvas.height

        if(needResize){
            renderer.setSize(width, height, false)
        }

        return needResize
    }
    
</script>
</body>
</html>

```
 <br>
<br>
outline pass
---------------

![image](https://user-images.githubusercontent.com/30430227/121863775-273aea80-cd37-11eb-9366-60d633ed1f5e.png)

```
<!DOCTYPE html>
<html lang="ko">
<head>
 <meta charset="UTF-8">
 <meta name="viewport" content="width=device-width, initial-scale=1.0">
 <meta http-equiv="X-UA-Compatible" content="ie=edge">
 <title>title</title>
 <style>
     *{
         margin: 0;
         padding: 0;
     }
     #c{
         display: block;
         width: 600px;
         height: 300px;
     }
 </style>
</head>
<body>
<canvas id="c"></canvas>

<script type="module">
    import * as THREE from './three.module.js'
    import {OrbitControls} from './OrbitControls.js'
    import {GLTFLoader} from './GLTFLoader.js'
    import Stats from './stats.module.js'

    import {EffectComposer} from './EffectComposer.js'
    import {RenderPass} from './RenderPass.js'
    import {OutlinePass} from './OutlinePass.js'

    let renderer, scene, camera, stats, controls, composer, outlineObjects

    const canvas = document.querySelector('#c')

    init()
    animate()

    function init(){
        renderer = new THREE.WebGLRenderer({canvas})
        renderer.shadowMap.enabled = true

        scene = new THREE.Scene()

        camera = new THREE.PerspectiveCamera(50, 2, .1, 100)
        camera.position.set(10,10,10)

        controls = new OrbitControls(camera, renderer.domElement)
        controls.enableDamping = true

        {
            const light = new THREE.SpotLight()
            light.position.set(15,15,15)
            light.castShadow = true
            light.shadow.mapSize.width = 1024
            light.shadow.mapSize.height = 1024
            scene.add(light)
        }

        const pickableObjects = new Array()

        const loader = new GLTFLoader()

        loader.load('../gltfs/simple.gltf',gltf=>{
            gltf.scene.traverse(child=>{
                if(child.isMesh){
                    let m = child
                    switch(m.name){
                        case 'Plane':
                            m.receiveShadow = true
                            break
                        default:
                            m.castShadow = true
                            pickableObjects.push(m)
                    }
                }
            })
            scene.add(gltf.scene)
        },xhr=>{
            console.log((xhr.loaded/xhr.total*100)+'% loaded')
        },err=>{
            console.log(err)
        })

        const raycaster = new THREE.Raycaster()
        const mouse = new THREE.Vector2()
        let intersects, intersectedObject
        
        document.addEventListener('mousemove', onDocumentMouseMove, false)
        const domRect = canvas.getBoundingClientRect()

        function addOutlineObject( object ) {
            outlineObjects = [];
            outlineObjects.push( object );
        }

        function onDocumentMouseMove(event){
            event.preventDefault()
            mouse.x = ((event.clientX - domRect.x)/canvas.clientWidth)*2 -1
            mouse.y = ((event.clientY - domRect.y)/canvas.clientHeight)*-2 +1
            raycaster.setFromCamera(mouse, camera)

            intersects = raycaster.intersectObjects(pickableObjects, false)

            if(intersects.length > 0){
                intersectedObject = intersects[0].object
                addOutlineObject(intersects[0].object)
            }else{
                intersectedObject = null
                outlinePass.selectedObjects = [];
            }

            pickableObjects.forEach((o,i)=>{
                if(intersectedObject && intersectedObject.name == o.name){
                    pickableObjects[i].scale.set(1.2,1.2,1.2)
                    outlinePass.selectedObjects = outlineObjects
                }else{
                    pickableObjects[i].scale.set(1,1,1)
                }
            })

        }

        composer = new EffectComposer(renderer)
        composer.addPass(new RenderPass(scene, camera))

        const outlinePass = new OutlinePass( new THREE.Vector2( canvas.clientWidth, canvas.clientHeight), scene, camera );
		composer.addPass( outlinePass );
        {
            outlinePass.edgeStrength = 5
            outlinePass.edgeGlow = .5
            outlinePass.edgeThickness = 1
            // outlinePass.pulsePeriod = 1
            outlinePass.visibleEdgeColor.set(0xff00ff)
            outlinePass.hiddenEdgeColor.set('yellow')

            const textureLoader = new THREE.TextureLoader();
				textureLoader.load( '../4g2.jpg', function ( texture ) {

					outlinePass.patternTexture = texture;
					texture.wrapS = THREE.RepeatWrapping;
					texture.wrapT = THREE.RepeatWrapping;

				} );
            outlinePass.usePatternTexture = true
        }

        stats = Stats()
        document.body.appendChild(stats.dom)
    }

    function resizeRendererToDisplaySize(renderer){
        const canvas = renderer.domElement
        const width = canvas.clientWidth
        const height = canvas.clientHeight

        const needResize = width!==canvas.width||height!==canvas.height

        if(needResize){
            renderer.setSize(width,height,false)
            composer.setSize(width,height,false)
        }

        return needResize
    }

    function animate(){
        if(resizeRendererToDisplaySize(renderer)){
            camera.aspect = canvas.width/canvas.height
            camera.updateProjectionMatrix()
        }
        
        controls.update()
        stats.update()
        
        // renderer.render(scene, camera)
        composer.render()
        
        requestAnimationFrame(animate)
    }
</script>
</body>
</html>
```
 <br>
<br>
# Clipping Face
-----------------

```
// 자동회전 스크립트
    clock = new THREE.Clock()
    ....
    const delta = clock.getDelta()
    meshs.rotation.y += delta*.5
    
// 스킨 머티리얼
        const textureLoader = new THREE.TextureLoader()
        const loader = new GLTFLoader()

        meshs = new THREE.Object3D()
        scene.add(meshs)

        loader.load('../gltfs/LeePerrySmith/LeePerrySmith.glb', gltf=>{
            const mesh = gltf.scene.children[0]

            mesh.material = new THREE.MeshPhongMaterial({
                specular: 0x111111,
                map: textureLoader.load('../gltfs/LeePerrySmith/Map-COL.jpg'),
                specularMap: textureLoader.load('../gltfs/LeePerrySmith/Map-SPEC.jpg'),
                normalMap: textureLoader.load('../gltfs/LeePerrySmith/Map-NOR.jpg'),
                shininess: 25,
            })

            meshs.add(mesh)
            scene.add(gltf.scene)
        },xhr=>{
            console.log((xhr.loaded/xhr.total*100)+'% loaded')
        }, err=>{
            console.log(err)
        })
// 평면헬퍼 PlaneHelper
        // Setup Plane(2D로 무한확장되는 평면)
        planes = [
            new THREE.Plane(new THREE.Vector3(-1,0,0), 50),
            new THREE.Plane(new THREE.Vector3(0,-1,0), 12),
            new THREE.Plane(new THREE.Vector3(0,0,-1), 50)
        ]
        //map은 해당 함수를 실행한 후 배열로 반환, map은 {}로 묶으면 안된다
        planeHelpers = planes.map(p=>new THREE.PlaneHelper(p, 50, 0xffffff))
        // planeHelpers = planes.map(p => new THREE.PlaneHelper(p, 50, 0xffffff));
        //foreach는 함수를 실행만 함
        planeHelpers.forEach(ph=>{
            ph.visible = false
            scene.add(ph)
        })
       
    
```
 <br>
<br>
Fresnel 쉐이더
---------------------

![image](https://user-images.githubusercontent.com/30430227/122662957-f8f35a00-d1d1-11eb-9c33-e629651aa938.png)

```
<!DOCTYPE html>
<html lang="ko">
<head>
 <meta charset="UTF-8">
 <meta name="viewport" content="width=device-width, initial-scale=1.0">
 <meta http-equiv="X-UA-Compatible" content="ie=edge">
 <title>title</title>
 <style>
     #c{
         display: block;
         width: 600px;
         height: 300px;
     }
 </style>
</head>
<body>
<canvas id="c"></canvas>

<script type="module">
    import * as THREE from './three.module.js'
    import {OrbitControls} from './OrbitControls.js'
    import {DecalGeometry} from './DecalGeometry.js'
    import {FresnelShader} from './FresnelShader.js'
    import {GLTFLoader} from './GLTFLoader.js'

    let renderer, scene, camera, controls, texture
    let mouse, raycaster, helper, decalMaterial

    const canvas = document.querySelector('#c')

    init()
    animate()

    function init(){
        renderer = new THREE.WebGLRenderer({canvas})
        scene = new THREE.Scene()
        camera = new THREE.PerspectiveCamera(75,2,.1,100)
        camera.position.set(0,0,5)

        controls = new OrbitControls(camera, renderer.domElement)
        
        const geometry = new THREE.SphereGeometry(2, 32, 16)

        const shader = FresnelShader
        const uniforms = THREE.UniformsUtils.clone(shader.uniforms)
        
        texture = new THREE.TextureLoader().load('../umbrellas.png',()=>{
            //큐브맵 크기 정의(이미지의 높이로)
            const rt = new THREE.WebGLCubeRenderTarget(texture.image.height)
            //정육면체 텍스처 생성
            rt.fromEquirectangularTexture(renderer, texture)
            scene.background = rt.texture
            uniforms['tCube'].value = rt.texture
        })


        const material = new THREE.ShaderMaterial({
            uniforms: uniforms,
            vertexShader:shader.vertexShader,
            fragmentShader:shader.fragmentShader
        })


        const mesh = new THREE.Mesh(geometry, material)

        scene.add(mesh)
    }

    function animate(){
        renderer.render(scene,camera)
        controls.update()
        
        requestAnimationFrame(animate)
    }
</script>
</body>
</html>
```
 <br>
<br>
TWEEN 애니메이션, 프로그래스바
-----------------------------

![image](https://user-images.githubusercontent.com/30430227/122923996-3d345500-d3a0-11eb-9972-d3e28f0e9f00.png)

```
<!DOCTYPE html>
<html lang="ko">
<head>
 <meta charset="UTF-8">
 <meta name="viewport" content="width=device-width, initial-scale=1.0">
 <meta http-equiv="X-UA-Compatible" content="ie=edge">
 <title>title</title>
 <style>
     body{
         overflow: hidden;
         margin: 0;
     }
     #progressBar{
         width: 500px;
         height: 24px;
         position: absolute;
         left: 50%;
         top: 25px;
         margin-left: -250px;
     }
     #instructions{
         color: white;
         background: #000;
         position: absolute;
         left: 50%;
         top: 10px;
         margin-left: -100px;
     }
 </style>
</head>
<body>
<progress id="progressBar" value="0" max="100"></progress>
<div id="instructions">더블클릭 하세따</div>

<script type="module">
    import * as THREE from './three.module.js'
    import {OrbitControls} from './OrbitControls.js'
    import {GLTFLoader} from './GLTFLoader.js'
    import {TWEEN} from './tween.module.min.js'

    const renderer = new THREE.WebGLRenderer()
    renderer.physicallyCorrectLights = true
    renderer.shadowMap.enabled = true
    renderer.outputEncoding = THREE.sRGBEncoding
    renderer.setSize(window.innerWidth, window.innerHeight)
    document.body.appendChild(renderer.domElement)

    const scene = new THREE.Scene()
    const axesHelper = new THREE.AxesHelper(5)
    scene.add(axesHelper)

    const camera = new THREE.PerspectiveCamera(75, window.innerWidth/window.innerHeight, .1,20)
    camera.position.set(0,2,5)
    const controls = new OrbitControls(camera, renderer.domElement)
    controls.endbleDamping = true
    controls.addEventListener('change', render)

    let sceneMeshes = new Array()
    let monkey

    const loader = new GLTFLoader()
    loader.load('../gltfs/Suzanne.gltf', gltf=>{
        gltf.scene.traverse(child=>{
            if(child.isMesh){
                let m = child
                m.castShadow = true
                m.receiveShadow = true
                if(child.name==='Plane'){
                    sceneMeshes.push(m)
                }else if(child.name==='Suzanne'){
                    monkey = m
                }
            }
            if(child.isLight){
                let l = child
                l.castShadow = true
                l.shadow.bias = -.001
                l.shadow.mapSize.width = 2048
                l.shadow.mapSize.height = 2048
            }
        })
        progressBar.style.display = 'none'
        scene.add(gltf.scene)
        render()
    },xhr=>{
            if(xhr.lengthComputable){
                progressBar.style.display='block'
                var percentComplete = xhr.loaded/xhr.total*100
                progressBar.value = percentComplete
            }
    },error=>console.log(error))

    window.addEventListener('resize', onWindowResize, false)
    function onWindowResize(){
        camera.aspect = window.innerWidth/window.innerHeight
        camera.updateProjectionMatrix()
        renderer.setSize(window.innerWidth, window.innerHeight)
        render()
    }

    const raycaster = new THREE.Raycaster()
    renderer.domElement.addEventListener('dblclick', onDoubleClick, false)
    function onDoubleClick(event){
        const mouse={
            x:(event.clientX/window.innerWidth)*2 -1,
            y:(event.clientY/window.innerHeight)*-2 +1 
        }
        raycaster.setFromCamera(mouse, camera)
        const intersects = raycaster.intersectObjects(sceneMeshes, false)
        if(intersects.length>0){
            const p = intersects[0].point
            new TWEEN.Tween(monkey.position)
                .to({x:p.x, z:p.z}, 500)
                // .easing(TWEEN.Easing.Bounce.Out)
                .start()
            new TWEEN.Tween(monkey.position)
                .to({y:p.y+3},250)
                .easing(TWEEN.Easing.Cubic.Out)
                .onUpdate(()=>render())
                .start()
                .onComplete(()=>{
                    new TWEEN.Tween(monkey.position)
                        .to({y:p.y+1},250)
                        .easing(TWEEN.Easing.Bounce.Out)
                        .onUpdate(()=>render())
                        .start()
                })
        }
    }

    function animate(){
        controls.update()
        TWEEN.update()
        render()
        requestAnimationFrame(animate)
    }
    animate()

    function render(){
        renderer.render(scene, camera)
    }

</script>
</body>
</html>
```
