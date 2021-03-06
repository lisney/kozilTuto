Babylon Basic
================


Basic Scene
---------------

![image](https://user-images.githubusercontent.com/30430227/173024992-7313dede-b2cb-4342-bec3-9907da6bbc79.png)

```
# css
body {
  width: 100%;
  height: 100%;
}

.box {
  margin: 0 auto;
  width: 70%;
  height: 70%;
}

#renderCanvas {
  width: 100%;
  height: 100%;
}


## babylonjs
const canvas = document.querySelector("#renderCanvas");
const engine = new BABYLON.Engine(canvas, true);

const scene = createScene();

engine.runRenderLoop(() => {
  scene.render();
});

function createScene() {
  const scene = new BABYLON.Scene(engine);

  const camera = new BABYLON.FreeCamera(
    "camera",
    new BABYLON.Vector3(0, 1, -5),
    scene
  );
  camera.setTarget(BABYLON.Vector3.Zero());
  camera.attachControl(canvas, false);
  camera.speed = 0.25; // 키보드 방향키 감도
  // camera.wheelPrecision = 50;//FreeCamera는 줌이 안된다

  const hemiLight = new BABYLON.HemisphericLight(
    "hemiLight",
    new BABYLON.Vector3(0, 1, 0),
    scene
  );

  hemiLight.intensity = 0.5;

  // Mesh
  const ground = BABYLON.MeshBuilder.CreateGround(
    "ground",
    { width: 10, height: 10 },
    scene
  );
  ground.material = CreateGroundMaterial();

  const ball = BABYLON.MeshBuilder.CreateSphere("ball", { diameter: 1 }, scene);
  ball.position = new BABYLON.Vector3(0, 1, 0);

  // Material
  function CreateGroundMaterial() {
    const groundMat = new BABYLON.StandardMaterial("groundMat", scene);
    const uvScale = 4;
    const texArray = [];

    const difTexture = new BABYLON.Texture(
      "./images/cobblestone_05_diff_1k.jpg",
      scene
    );
    groundMat.diffuseTexture = difTexture;
    texArray.push(difTexture);

    const norTexture = new BABYLON.Texture(
      "./images/cobblestone_05_nor_gl_1.jpg",
      scene
    );
    groundMat.bumpTexture = norTexture;
    groundMat.invertNormalMapX = true;
    groundMat.invertNormalMapY = true;
    texArray.push(norTexture);

    const aoTexture = new BABYLON.Texture(
      "./images/cobblestone_05_rough_1k.jpg",
      scene
    );
    groundMat.ambientTexture = aoTexture;
    texArray.push(aoTexture);

    const spTexture = new BABYLON.Texture(
      "./images/cobblestone_05_rough_1k.jpg",
      scene
    );
    groundMat.specularTexture = spTexture;
    groundMat.specularPower = 10;
    texArray.push(spTexture);

    texArray.forEach((tex) => {
      tex.uScale = uvScale;
      tex.vScale = uvScale;
    });

    return groundMat;
  }

  return scene;
}
```



PBR Material 
-------------------

![image](https://user-images.githubusercontent.com/30430227/173498961-c6db96cb-34fe-466b-93bc-2e899e231cf9.png)

```
const canvas = document.querySelector("#renderCanvas");
const engine = new BABYLON.Engine(canvas, true);

const scene = createScene();

engine.runRenderLoop(() => {
  scene.render();
});

function createScene() {
  const scene = new BABYLON.Scene(engine);
  // scene.environmentIntensity = 0.1;//씬 환경 텍스처 Intensity

  const camera = new BABYLON.ArcRotateCamera(
    "camera",
    Math.PI / 4,
    Math.PI / 3,
    5,
    new BABYLON.Vector3(0, 0, 0),
    scene
  );
  // camera.setTarget(BABYLON.Vector3.Zero());//freeCamera용 타깃
  camera.attachControl(canvas, false);
  // camera.speed = 0.25; // 키보드 방향키 감도
  camera.wheelPrecision = 50; //FreeCamera는 줌이 안된다

  const hemiLight = new BABYLON.HemisphericLight(
    "hemiLight",
    new BABYLON.Vector3(0, 1, 1),
    scene
  );

  hemiLight.intensity = 0.5;

  // Mesh
  const ground = BABYLON.MeshBuilder.CreateGround(
    "ground",
    { width: 5, height: 5 },
    scene
  );

  const ball = BABYLON.MeshBuilder.CreateSphere("ball", { diameter: 2 }, scene);
  ball.position = new BABYLON.Vector3(0, 1, 0);

  // Material
  function CreateGroundMaterial() {
    const pbr = new BABYLON.PBRMaterial("pbr", scene);
    pbr.roughness = 1; //값이 0 면 거울
    pbr.albedoTexture = new BABYLON.Texture("./images/adiffuse.jpg", scene);
    pbr.albedoTexture.vScale = 4;
    pbr.albedoTexture.uScale = 4;
    pbr.bumpTexture = new BABYLON.Texture("./images/anormal.jpg", scene);
    pbr.bumpTexture.vScale = 4;
    pbr.bumpTexture.uScale = 4;
    pbr.invertNormalMapX = true;
    pbr.invertNormalMapY = true;
    pbr.emissiveTexture = new BABYLON.Texture("./images/aambientt.jpg", scene); //흰색이 Emissive, 검정은 투명
    pbr.emissiveColor = new BABYLON.Color3(1, 1, 1);
    // pbr.emissiveIntensity = 3;

    const glowLayer = new BABYLON.GlowLayer("glow", scene);
    glowLayer.intensity = 1;

    // pbr.useAmbientOcclusionFromMetallicTextureRed = true;
    // pbr.useRoughnessFromMetallicTextureGreen = true;
    // pbr.useMetallinessFromMetallicTextureBlue = true;

    return pbr;
  }
  ground.material = CreateGroundMaterial();

  function CreateBallMaterial() {
    const pbr = new BABYLON.PBRMetallicRoughnessMaterial("pbr", scene);
    pbr.baseColor = new BABYLON.Color3(1, 0.7, 0.3);
    pbr.metallic = 1;
    pbr.roughness = 0;
    pbr.environmentTexture = BABYLON.CubeTexture.CreateFromPrefilteredData(
      "images/environment.dds",
      scene
    );
    return pbr;
  }

  ball.material = CreateBallMaterial();

  const envTexture = new BABYLON.CubeTexture("images/environment.env", scene);
  scene.createDefaultSkybox(envTexture, true, 1000, 0.2); //0.2 Texture Roughness

  return scene;
}

```


import GLB(GLTF)
-------------------

![image](https://user-images.githubusercontent.com/30430227/173505758-a0ff0ed1-7246-4bd0-8390-3f5ce88a9470.png)

```
  BABYLON.SceneLoader.ImportMesh(
    "",
    "./gltfs/",
    "afo.gltf",
    scene,
    (meshes) => {
      scene.meshes[0].position = new BABYLON.Vector3(0, -1, 0);
    }
  );
```


그림자와 Gizmo
--------------

![image](https://user-images.githubusercontent.com/30430227/173759157-b837b499-1e32-4dcd-b6a4-40875e3aae41.png)

`블랜더에서 라이트 제거하고 export`

```
  const camera = new BABYLON.FreeCamera(
    "camera",
    new BABYLON.Vector3(5, 5, -5),
    scene
  );
  camera.setTarget(BABYLON.Vector3.Zero()); //freeCamera용 타깃
  camera.attachControl(canvas, false);
  camera.speed = 0.25; // 키보드 방향키 감도

  //조명

  // const hemiLight = new BABYLON.HemisphericLight(
  //   "hemiLight",
  //   new BABYLON.Vector3(0, 1, 0),
  //   scene
  // );
  // hemiLight.intensity = 0.5;
  // hemiLight.diffuse = new BABYLON.Color3(1, 0, 0);
  // hemiLight.groundColor = new BABYLON.Color3(0, 0, 1);
  // hemiLight.specular = new BABYLON.Color3(0, 1, 0);

  const spotLight = new BABYLON.SpotLight(
    "spotLight",
    new BABYLON.Vector3(-3, 3, -5), //위치
    new BABYLON.Vector3(3, -2, 3), //방향-위치가 아니라 방향이다
    Math.PI / 2, //조명 각도
    10, //Exponent
    scene
  );
  spotLight.intensity = 10;
  spotLight.shadowEnabled = true;
  spotLight.shadowMinZ = 1;//shadow clipping minimum
  spotLight.shadowMaxZ = 10;

  //shadow 제너레이터
  const shadowGen = new BABYLON.ShadowGenerator(1024, spotLight);
  shadowGen.useBlurCloseExponentialShadowMap = true;//부드러운 가장자리

  CreateGizmos(spotLight);

  // Light Gizmo
  function CreateGizmos(customLight) {
    const lightGizmo = new BABYLON.LightGizmo(); //라이트 메쉬
    lightGizmo.scaleRatio = 3;
    lightGizmo.light = customLight;

    const gizmoManager = new BABYLON.GizmoManager(scene); //컨트롤러
    gizmoManager.positionGizmoEnabled = true;
    gizmoManager.rotationGizmoEnabled = true;
    gizmoManager.usePointerToAttachGizmos = false;
    gizmoManager.scaleRatio = 2;
    gizmoManager.attachToMesh(lightGizmo.attachedMesh);
  }

  // Import GLB file
  BABYLON.SceneLoader.ImportMesh(
    "",
    "./gltfs/",
    "nolight.glb",
    scene,
    (meshes) => {
      meshes.map((mesh) => {
        mesh.receiveShadows = true;
        shadowGen.addShadowCaster(mesh);
      });
    }
  );
```

블랜더 bake shadow map
----------------------

1. UV Editor > New image > Save

![image](https://user-images.githubusercontent.com/30430227/173763939-58359107-e8d3-48a2-8e59-9f4ab71b4319.png)
![image](https://user-images.githubusercontent.com/30430227/173763872-fad99dda-aa68-4fdb-871e-48baf1f3b545.png)

![image](https://user-images.githubusercontent.com/30430227/173764558-97b03eaf-da13-478b-9caa-8caf93fb91a8.png)


2. 수지와 바닥 각각에 UV Map 추가 'Shadow' > Edit Mode > Pack Islands

![image](https://user-images.githubusercontent.com/30430227/173765867-029682bf-5dd7-4e86-a95c-a1d56baa92e9.png)
![image](https://user-images.githubusercontent.com/30430227/173765914-639d6e90-fbf7-4609-a4dc-638b9197f5df.png)


3. Shader Editor > Image Texture노드 생성 후 shadow 이미지 선택 > 선택상태로 둔다 > 복사한 후 바닥 쉐이더에 붙여넣기

![image](https://user-images.githubusercontent.com/30430227/173767216-478b587f-eb7c-4fe4-a4ee-52e35b2d951c.png)

`Cycles Soft Shadow - Radius 값을 높인다`

![image](https://user-images.githubusercontent.com/30430227/173769356-6a38c1fe-2e88-44f5-8eb7-9161b4db2a75.png)


4. 베이크 - 수지와 바닥 선택 > Margin : 8px > Bake

![image](https://user-images.githubusercontent.com/30430227/173770645-15de3595-464f-4f69-a0a6-b91e4feaf7f5.png)
![image](https://user-images.githubusercontent.com/30430227/173770572-39f3c648-1aeb-4016-9875-33f5ec1595a0.png)


5. Compositor Editor > Rendering - 노이즈, 색상 자연스럽게..> 이미지 저장 jpg

![image](https://user-images.githubusercontent.com/30430227/173771161-c58c4029-1aa9-4663-8245-cfc5b9acdaed.png)

![image](https://user-images.githubusercontent.com/30430227/173774065-ce3a8a4e-2c5f-49d3-9077-054c0972a4cf.png)

`참고-기본 씬 대신 랜더`

![image](https://user-images.githubusercontent.com/30430227/173779487-80511d9b-3648-4301-a17a-a012d3ee240d.png)



6. Shader Editor

![image](https://user-images.githubusercontent.com/30430227/173776365-1c766161-c171-48d4-a25c-70b935f9e572.png)

![image](https://user-images.githubusercontent.com/30430227/173776421-d015316f-a059-48f6-8a3f-0c5d27d5d1a3.png)


7. Export

![image](https://user-images.githubusercontent.com/30430227/173776733-14434d0a-3b7d-4d2d-a887-959e7165bb19.png)


8. Babylon

![image](https://user-images.githubusercontent.com/30430227/173777429-c35f4689-c3c6-46a9-92e1-90284d933d59.png)

```
  const camera = new BABYLON.FreeCamera(
    "camera",
    new BABYLON.Vector3(0, 1, 1),
    scene
  );
  camera.setTarget(BABYLON.Vector3.Zero()); //freeCamera용 타깃
  camera.attachControl(canvas, false);
  camera.speed = 0.1; // 키보드 방향키 감도
  camera.minZ = 0.01; //Cliping Planes min
```


로딩스크린 - splash
----------------------

![image](https://user-images.githubusercontent.com/30430227/174218133-51cf3486-b364-4da1-9485-c6c4d76396c3.png)

```
# home.hbs
<div class="box">
    <div id="loader">
        <p>Loading</p>
        <div id="loadingContainer">
            <div id="loadingBar">로딩바다</div>
        </div>
        <p id="percentLoaded">25%</p>
    <H2>Loading</p>
    </div>
<canvas id="renderCanvas"></canvas>

</div>

<script src="babylon.js"></script>

# style.css
body {
  width: 100%;
  height: 100%;
}

.box {
  margin: 0 auto;
  width: 70%;
  height: 70%;
}

#renderCanvas {
  width: 100%;
  height: 100%;
}

#loader {
  width: 100%;
  height: 100%;
  background: slategray;
  font-size: 1rem;
  position: absolute;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

#loaded {
  width: 100%;
  height: 100%;
  color: white;
  background: slategray;
  position: absolute;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  opacity: 0;
  transition: opacity 1s ease;
}

#loadingBar {
  width: 10%;
  height: 2rem;
  background: white;
}

#loadingContainer {
  width: 50%;
  display: flex;
  justify-content: center;
  align-items: center;
}

p {
  width: 15%;
  font-size: 6rem;
  font-family: "Oswald";
  font-weight: 500;
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 0;
  margin: 0;
}

p:nth-child(1) {
  /* color: rgb(0, 129, 70); */
  animation: pulse 0.5s alternate infinite ease-in-out;
  font-weight: 400;
  justify-content: flex-end;
}
p:nth-child(2) {
  font-size: 5.25rem;
  animation: pulseFade 0.5s alternate infinite ease-in-out;
}
p:nth-child(3) {
  /* color: rgb(0, 129, 70); */
  color: white;
  justify-content: flex-start;
  animation: pulse 0.5s alternate infinite ease-in-out 0.5s;
}

h2 {
  margin: 0.25rem;
  font-family: "Oswald";
  font-weight: 200;
}

@keyframes pulse {
  to {
    transform: scale(0.8);
    opacity: 0.5;
  }
}

@keyframes pulseFade {
  to {
    opacity: 0.5;
  }
}

# babylon.js
const canvas = document.querySelector("#renderCanvas");

//로딩바
const loadingBar = document.querySelector("#loadingBar");
const percentLoaded = document.querySelector("#percentLoaded");
const loader = document.querySelector("#loader");

//엔진
const engine = new BABYLON.Engine(canvas, true);

class CustomLoadingScreen {
  constructor(loadingBar, percentLoaded, loader) {
    //constructor - Class에서 프로퍼티(변수)를 선언하는 곳
    this.loadingBar = loadingBar;
    this.percentLoaded = percentLoaded;
    this.loader = loader;
    this.status = 0;
  }
  displayLoadingUI() {
    this.loadingBar.style.width = "100%";
    this.percentLoaded.innerText = "0%";
    // alert(this.loadingUIText);
    console.log("로딩중");
  }
  hideLoadingUI() {
    this.loader.id = "loaded";

    setTimeout(() => {
      this.loader.style.display = "none";
    }, 1000);
    // alert("니주글래!");
  }
  updateLoadStatus(status) {
    //status - ImportMeshAsync의 evt 값 받음
    this.loadingBar.style.width = `${status}%`;
    this.percentLoaded.innerText = `${status}%`;
    console.log(status);
  }
}

const loadingScreen = new CustomLoadingScreen(
  loadingBar,
  percentLoaded,
  loader
);

engine.loadingScreen = loadingScreen;

const scene = createScene();

engine.runRenderLoop(() => {
  scene.render();
  engine.hideLoadingUI();
});

function createScene() {
  const scene = new BABYLON.Scene(engine);

  engine.displayLoadingUI();

  const camera = new BABYLON.FreeCamera(
    "camera",
    new BABYLON.Vector3(0, 1, 1),
    scene
  );
  camera.setTarget(BABYLON.Vector3.Zero()); //freeCamera용 타깃
  camera.attachControl(canvas, false);
  camera.speed = 0.1; // 키보드 방향키 감도
  camera.minZ = 0.01; //Cliping Planes min

  // Import GLB file
  BABYLON.SceneLoader.ImportMeshAsync(
    "",
    "./gltfs/",
    "suzie.glb",
    scene,
    (evt) => {
      // evt는 ...Async로 부를 때 생성
      const loadStatus = ((evt.loaded * 100) / evt.total).toFixed();
      loadingScreen.updateLoadStatus(loadStatus);
    }
  );

  const envTexture = new BABYLON.CubeTexture("images/environment.env", scene);
  scene.createDefaultSkybox(envTexture, true, 1000, 0.2);

  return scene;
}
```


Camera Behavior - 비동기 방식
------------------

![image](https://user-images.githubusercontent.com/30430227/174257419-c63f95f2-68dc-4af2-a909-397d8e1ca6ac.png)

```
# 초기 CSS, HTML로
const canvas = document.querySelector("#renderCanvas");
const engine = new BABYLON.Engine(canvas, true);

// const scene = createScene();

// engine.runRenderLoop(() => {
//   scene.render();
// });

// 비동기 버전
function startRenderLoop(sceneToRender) {
  engine.runRenderLoop(() => {
    sceneToRender.render();
  });
}

createScene().then(startRenderLoop);

async function createScene() {
  const scene = new BABYLON.Scene(engine);

  const camera = new BABYLON.ArcRotateCamera(
    "camera",
    Math.PI / 2, //좌우회전 > 반시계방향
    Math.PI / 3, //상하회전 > X축 기준 반시계
    5, //Radius - 거리
    new BABYLON.Vector3.Zero(),
    scene
  );
  camera.attachControl(canvas, false);
  camera.wheelPrecision = 50;
  camera.minZ = 0.1; //clipping Plane

  //Radius -거리 한계
  camera.lowerRadiusLimit = 0.5;
  camera.upperRadiusLimit = 5;

  //panning Sensibility - 0이면 No Pan, 500 - 기본값
  camera.panningSensibility = 0;

  // 비헤이비어
  // camera.useBouncingBehavior = true; // Radius 한계에 오면 팅김
  camera.useAutoRotationBehavior = true; //클릭하면 멈춤
  camera.autoRotationBehavior.idleRotationSpeed = 0.5;//음수 - 반대방향 회전
  camera.autoRotationBehavior.idleRotationSpinupTime = 1000; //Ease In
  camera.autoRotationBehavior.idleRotationWaitTime = 2000; //클릭 멈춤 후 대기시간
  camera.autoRotationBehavior.zoomStopsAnimation = true; //Wheel - Zoom 때도 회전 멈춤

  // 프레임 비헤이비어 - 화면 사이즈 변형 애니
  camera.useFramingBehavior = true;
  camera.framingBehavior.radiusScale = 1;
  camera.framingBehavior.framingTime = 4000; //사이즈 변경 시간

  // Import GLB
  // 일반 glb에서는 콜백함수로 meshes 사용, 로딩스크린 evt 가 되지 않는다
  // BABYLON.SceneLoader.ImportMesh(
  //   "",
  //   "./gltfs/",
  //   "suzie.glb",
  //   scene,
  //   (meshes) => {
  //     camera.setTarget(meshes[0]);
  //   }
  // );

  // 비동기 버전 GLB
  const { meshes } = await BABYLON.SceneLoader.ImportMeshAsync(
    "",
    "./gltfs/",
    "suzie.glb",
    scene
  );

  // const watch = meshes[0];
  const watch = scene.getMeshByName("Cylinder"); //meshes[0]
  watch.showBoundingBox = true;
  camera.setTarget(watch);

  const envTexture = new BABYLON.CubeTexture("images/environment.env", scene);
  scene.createDefaultSkybox(envTexture, true, 100, 0.1);

  return scene;
}
```


Mesh Action Manager
--------------

![image](https://user-images.githubusercontent.com/30430227/174418626-893f13c7-0488-4b8c-9c4f-ba37a032c655.png)

```

  //재질
  const sphereMat = new BABYLON.PBRMaterial("sphereMat", scene);
  sphereMat.albedoColor = new BABYLON.Color3(1, 0, 0);
  sphereMat.roughness = 0;

  // const watch = meshes[0];
  const watch = scene.getMeshByName("Cylinder"); //meshes[0]
  watch.showBoundingBox = true;
  camera.setTarget(watch);
  meshes[1].material = sphereMat;

  const envTexture = new BABYLON.CubeTexture("images/environment.env", scene);
  scene.createDefaultSkybox(envTexture, true, 100, 0.1);

  //회전용 큐브
  const cube = new BABYLON.MeshBuilder.CreateBox("cube", {
    height: 0.5,
    width: 0.5,
    depth: 0.5,
  });
  cube.material = sphereMat;

  //메시 액션
  //interpolate..부드러운 액션
  function CreateAction() {
    watch.actionManager = new BABYLON.ActionManager(scene);
    watch.actionManager.registerAction(
      new BABYLON.SetValueAction(
        BABYLON.ActionManager.OnPickDownTrigger,
        watch,
        "scaling",
        new BABYLON.Vector3(1.2, 1.2, 1.2)
      )
    );

    watch.actionManager
      .registerAction(
        new BABYLON.InterpolateValueAction(
          BABYLON.ActionManager.OnPickDownTrigger,
          sphereMat,
          "roughness",
          1,
          3000
        )
      )
      .then(
        //한 번 더 클릭하면 실행
        new BABYLON.InterpolateValueAction(
          BABYLON.ActionManager.NothingTrigger,
          sphereMat,
          "roughness",
          0,
          1000
        )
      );

    scene.actionManager = new BABYLON.ActionManager(scene);
    scene.actionManager.registerAction(
      new BABYLON.IncrementValueAction(
        BABYLON.ActionManager.OnEveryFrameTrigger,
        cube,
        "rotation.y",
        0.01
      )
    );
  }

  CreateAction();

```


First Person Controller
---------------------------

![image](https://user-images.githubusercontent.com/30430227/174420446-05a973b6-10e1-48ba-916c-43b9cd2e30cf.png)

```
const canvas = document.querySelector("#renderCanvas");
const engine = new BABYLON.Engine(canvas, true);

// 비동기 버전
function startRenderLoop(sceneToRender) {
  engine.runRenderLoop(() => {
    sceneToRender.render();
  });
}

createScene().then(startRenderLoop);

async function createScene() {
  const scene = new BABYLON.Scene(engine);

  const camera = new BABYLON.FreeCamera(
    "camera",
    new BABYLON.Vector3(0, 1, 0),
    scene
  );
  camera.attachControl(canvas, false);
  camera.applyGravity = true;
  camera.checkCollisions = true;
  camera.ellipsoid = new BABYLON.Vector3(1, 1, 1);
  camera.minZ = 0.1;
  camera.speed = 0.75;
  camera.angularSensibility = 4000; //카메라 회전 감도, 낮을 수록 높다

  //라이트
  new BABYLON.HemisphericLight("hemi", new BABYLON.Vector3(0, 1, 0), scene);
  
  // 마우스 포인터 락 - ESC키 해제
  scene.onPointerDown = (evt) => {
    if (evt.button === 0) engine.enterPointerlock();
    if (evt.button === 1) engine.exitPointerlock();
  };

  const framesPerSecond = 60;
  const gravity = -9.81;
  scene.gravity = new BABYLON.Vector3(0, gravity / framesPerSecond, 0);
  scene.collisionsEnabled = true;

  // 비동기 버전 GLB
  const { meshes } = await BABYLON.SceneLoader.ImportMeshAsync(
    "",
    "./gltfs/",
    "fpc.glb",
    scene
  );
  meshes.map((mesh) => {
    mesh.checkCollisions = true;
  });

  //재질
  const sphereMat = new BABYLON.PBRMaterial("sphereMat", scene);
  sphereMat.albedoColor = new BABYLON.Color3(1, 0, 0);
  sphereMat.roughness = 0;

  return scene;
}
```


Physics Impostor-사칭자
----------------------------

![image](https://user-images.githubusercontent.com/30430227/174461610-012890ee-97db-4b16-b1d4-884b14721085.png)

![image](https://user-images.githubusercontent.com/30430227/174463649-0372437e-707d-499f-8088-138f7fc43aec.png)

```
# main.hbs
  <script src="https://cdnjs.cloudflare.com/ajax/libs/cannon.js/0.6.2/cannon.min.js"></script>
  
# babylon.js
const canvas = document.querySelector("#renderCanvas");
const engine = new BABYLON.Engine(canvas, true);

// 비동기 버전
function startRenderLoop(sceneToRender) {
  engine.runRenderLoop(() => {
    sceneToRender.render();
  });
}

createScene().then(startRenderLoop);

async function createScene() {
  const scene = new BABYLON.Scene(engine);

  const camera = new BABYLON.FreeCamera(
    "camera",
    new BABYLON.Vector3(0, 1, -5),
    scene
  );
  camera.attachControl(canvas, false);
  camera.applyGravity = true;
  camera.checkCollisions = true;
  camera.ellipsoid = new BABYLON.Vector3(1, 1, 1);
  camera.minZ = 0.1;
  camera.speed = 0.75;
  camera.angularSensibility = 4000; //카메라 회전 감도, 낮을 수록 높다

  camera.setTarget(BABYLON.Vector3.Zero());

  //라이트
  new BABYLON.HemisphericLight("hemi", new BABYLON.Vector3(0, 1, 0), scene);

  // 마우스 포인터 락 - ESC키 해제
  scene.onPointerDown = (evt) => {
    if (evt.button === 0) engine.enterPointerlock();
    if (evt.button === 1) engine.exitPointerlock();
  };

  const framesPerSecond = 60;
  const gravity = -9.81;
  scene.gravity = new BABYLON.Vector3(0, gravity / framesPerSecond, 0);
  scene.collisionsEnabled = true;

  //재질
  function randColor() {
    randMat = new BABYLON.PBRMaterial("randMat", scene);
    randMat.albedoColor = new BABYLON.Color3(
      Math.random(),
      Math.random(),
      Math.random
    );
    randMat.roughness = 1;

    return randMat;
  }

  // 비동기 버전 GLB
  const { meshes } = await BABYLON.SceneLoader.ImportMeshAsync(
    "",
    "./gltfs/",
    "fpc.glb",
    scene
  );

  meshes.map((mesh) => {
    mesh.checkCollisions = true;
    mesh.material = randColor();
  });

  //물리 사칭자(Impostor)
  const sphere = BABYLON.MeshBuilder.CreateSphere(
    "sphere",
    { diameterX: 1, segments: 4 },
    scene
  );
  sphere.position.y = 8;

  const box = BABYLON.MeshBuilder.CreateBox("box", { size: 1 });
  box.position = new BABYLON.Vector3(0, 3, 0);

  const ground = BABYLON.MeshBuilder.CreateGround("ground", {
    width: 10,
    height: 10,
  });
  ground.position.y = -1;
  ground.isVisible = false; //랜더 시 안보이게

  scene.enablePhysics(null, new BABYLON.CannonJSPlugin()); //중력 null = new BABYLON.Vector3(0,-9.8,0)

  sphere.physicsImpostor = new BABYLON.PhysicsImpostor(
    sphere,
    BABYLON.PhysicsImpostor.SphereImpostor,
    { mass: 1, restitution: 0.9, friction: 0.5 },
    scene
  );

  box.physicsImpostor = new BABYLON.PhysicsImpostor(
    box,
    BABYLON.PhysicsImpostor.BoxImpostor,
    { mass: 1, resititution: 0.5 }
  );

  ground.physicsImpostor = new BABYLON.PhysicsImpostor(
    ground,
    BABYLON.PhysicsImpostor.BoxImpostor,
    { mass: 0, resitution: 0.9 }, //mass 0 - 고정
    scene
  );

  // 물리 충돌
  box.physicsImpostor.registerOnPhysicsCollide(
    sphere.physicsImpostor,
    detectCollisions
  );

  function detectCollisions(boxCol, colAgainst) {
    //boxCol 맞은 놈, colAgainst 때린 놈, 배열[] 가능
    boxCol.object.scaling = new BABYLON.Vector3(1.5, 0.5, 1.5); //충돌하면 3배로 커진다
    boxCol.setScalingUpdated();

    const redMat = new BABYLON.StandardMaterial("redMat", scene);
    redMat.diffuseColor = new BABYLON.Color3(1, 0, 0);
    colAgainst.object.material = redMat;
  }

  // 트리거 테스트
  scene.registerBeforeRender(() => {
    //랜더링 할 때마다 호출 engine.runRenderLoop
    console.log("intersects?", box.intersectsMesh(sphere));
  });

  return scene;
}
```


Physics Velocity 
-------------------

![image](https://user-images.githubusercontent.com/30430227/174565648-94cc2e6e-d860-4e92-9098-de04b8d96c0a.png)

```
# Camera
  const camera = new BABYLON.FreeCamera(
    "camera",
    new BABYLON.Vector3(0, 1, -5),
    scene
  );
  camera.attachControl(canvas, false);
  // camera.applyGravity = true;
  // camera.checkCollisions = true;
  // camera.ellipsoid = new BABYLON.Vector3(1, 1, 1);
  camera.minZ = 0.1;
  camera.speed = 0.75;
  camera.angularSensibility = 4000; //카메라 회전 감도, 낮을 수록 높다

  camera.setTarget(BABYLON.Vector3.Zero());
  
# GLBs
  // 비동기 버전 GLB
  const { meshes: meshA } = await BABYLON.SceneLoader.ImportMeshAsync(
    //{}구조분해할당 별명 :meshA
    "",
    "./gltfs/",
    "fpc.glb",
    scene
  );

  meshA.map((mesh) => {
    mesh.checkCollisions = true;
    mesh.material = randColor();
  });

  const { meshes: meshB } = await BABYLON.SceneLoader.ImportMeshAsync(
    "",
    "./gltfs/",
    "suzi.glb",
    scene
  );

  scene.enablePhysics(null, new BABYLON.CannonJSPlugin()); //중력 null = new BABYLON.Vector3(0,-9.8,0)

  const suziCol = BABYLON.MeshBuilder.CreateBox("suziCol", {
    width: 2,
    height: 2,
    depth: 2,
  });
  suziCol.position.y = 2;

  suziCol.visibility = 0.25;

  suziCol.physicsImpostor = new BABYLON.PhysicsImpostor(
    suziCol,
    BABYLON.PhysicsImpostor.BoxImpostor,
    { mass: 1 },
    scene
  );

  // 회전
  suziCol.rotate(BABYLON.Vector3.Forward(), 1.5); //라디안 값 1.5

  //부모
  meshB[0].parent = suziCol; //자식의 위치 이동>부모로
  // meshB[0].setParent(suziCol);//자식 위치 그대로

  //바닥
  const ground = BABYLON.MeshBuilder.CreateGround("ground", {
    width: 10,
    height: 10,
  });
  ground.position.y = -1;
  ground.isVisible = false; //랜더 시 안보이게

  ground.physicsImpostor = new BABYLON.PhysicsImpostor(
    ground,
    BABYLON.PhysicsImpostor.BoxImpostor,
    { mass: 0, resitution: 0.9 }, //mass 0 - 고정
    scene
  );

  // 물리 속도-추진력 (1)번 시리즈 테스트 후 (2)번 라인들 테스트
  function suziPhysics() {
    // suziCol.physicsImpostor.setLinearVelocity(new BABYLON.Vector3(0, 1, 0));//(1)
    suziCol.physicsImpostor.setLinearVelocity(suziCol.up.scale(2)); //(2) .scale() 배속
    // suziCol.physicsImpostor.setAngularVelocity(new BABYLON.Vector3(0, 1, 0)); //(1) 회전 추진
    suziCol.physicsImpostor.setAngularVelocity(suziCol.up); //(2)회전 추진
    // camera.position.y = suziCol.position.y;//(1)
    camera.position = new BABYLON.Vector3( //(2)
      suziCol.position.x,
      suziCol.position.y,
      camera.position.z
    );
  }

  //매 프레임마다 실행
  scene.registerBeforeRender(suziPhysics);

  //마우스 클릭 시onPointerDown 물리 끔 unregisterBe..
  let gameOver = false;

  if (!gameOver) scene.registerBeforeRender(suziPhysics);

  scene.onPointerDown = () => {
    gameOver = true;
    scene.unregisterBeforeRender(suziPhysics);
  };
```



Physics Force
--------------

![image](https://user-images.githubusercontent.com/30430227/174596728-e2cee0bf-9d32-40fc-aa60-dfacec54251f.png)

```
const canvas = document.querySelector("#renderCanvas");
const engine = new BABYLON.Engine(canvas, true);

// 비동기 버전
function startRenderLoop(sceneToRender) {
  engine.runRenderLoop(() => {
    sceneToRender.render();
  });
}

createScene().then(startRenderLoop);

async function createScene() {
  const scene = new BABYLON.Scene(engine);

  const camera = new BABYLON.FreeCamera(
    "camera",
    new BABYLON.Vector3(0, 1, -5),
    scene
  );
  camera.attachControl(canvas, false);
  // camera.applyGravity = true;
  // camera.checkCollisions = true;
  // camera.ellipsoid = new BABYLON.Vector3(1, 1, 1);
  camera.minZ = 0.1;
  camera.speed = 0.75;
  camera.angularSensibility = 4000; //카메라 회전 감도, 낮을 수록 높다

  camera.setTarget(BABYLON.Vector3.Zero());

  //라이트
  new BABYLON.HemisphericLight("hemi", new BABYLON.Vector3(0, 1, 0), scene);

  // 마우스 포인터 락
  // scene.onPointerDown = (evt) => {
  //   if (evt.button === 0) engine.enterPointerlock();
  //   if (evt.button === 1) engine.exitPointerlock();
  // };

  const framesPerSecond = 60;
  const gravity = -9.81;
  scene.gravity = new BABYLON.Vector3(0, gravity / framesPerSecond, 0);
  scene.collisionsEnabled = true;

  //재질
  function randColor() {
    randMat = new BABYLON.PBRMaterial("randMat", scene);
    randMat.albedoColor = new BABYLON.Color3(
      Math.random(),
      Math.random(),
      Math.random
    );
    randMat.roughness = 1;

    return randMat;
  }

  // 비동기 버전 GLB
  const { meshes: meshA } = await BABYLON.SceneLoader.ImportMeshAsync(
    //{}구조분해할당 별명 :meshA
    "",
    "./gltfs/",
    "fpc.glb",
    scene
  );

  meshA.map((mesh) => {
    mesh.checkCollisions = true;
    mesh.material = randColor();
  });

  const { meshes: meshB } = await BABYLON.SceneLoader.ImportMeshAsync(
    "",
    "./gltfs/",
    "suzi.glb",
    scene
  );

  scene.enablePhysics(
    new BABYLON.Vector3(0, -9.81, 0),
    new BABYLON.CannonJSPlugin()
  );

  // 박스
  const box = BABYLON.MeshBuilder.CreateBox("box", {
    size: 2,
  });
  const boxMat = new BABYLON.PBRMaterial("boxMat", scene);
  boxMat.albedoColor = new BABYLON.Color3(1, 0.5, 0);
  boxMat.roughness = 1;

  box.material = boxMat;

  box.physicsImpostor = new BABYLON.PhysicsImpostor(
    box,
    BABYLON.PhysicsImpostor.BoxImpostor,
    { mass: 1, friction: 1 },
    scene
  );

  // 액션 매니저 - 마우스 충격파!
  box.actionManager = new BABYLON.ActionManager(scene);

  box.actionManager.registerAction(
    new BABYLON.ExecuteCodeAction(
      BABYLON.ActionManager.OnPickDownTrigger,
      () => {
        box.physicsImpostor.applyImpulse(
          new BABYLON.Vector3(0, 0, 5),
          box.getAbsolutePosition().add(new BABYLON.Vector3(0, 1, 0))
        ); //getAbsolutePosition 센터포지션, .add() 추가하면 센터 + Y1 높이가 펄스 포인트
      }
    )
  );

  //바닥
  const ground = BABYLON.MeshBuilder.CreateGround("ground", {
    width: 40,
    height: 40,
  });
  ground.position.y = -1;
  ground.isVisible = false; //랜더 시 안보이게

  ground.physicsImpostor = new BABYLON.PhysicsImpostor(
    ground,
    BABYLON.PhysicsImpostor.BoxImpostor,
    { mass: 0, resitution: 0.9 }, //mass 0 - 고정
    scene
  );

  // 캐논 볼

  let cannonball;
  function CreateCannonball() {
    cannonball = BABYLON.MeshBuilder.CreateSphere("cannonball", {
      diameter: 0.5,
    });
    const ballMat = new BABYLON.PBRMaterial("ballMat", scene);
    ballMat.albedoColor = new BABYLON.Color3(1, 0, 0);
    ballMat.roughness = 1;

    cannonball.material = ballMat;

    cannonball.physicsImpostor = new BABYLON.PhysicsImpostor(
      cannonball,
      BABYLON.PhysicsImpostor.SphereImpostor,
      { mass: 1, friction: 1 }
    );
    cannonball.position = camera.position;
    cannonball.setEnabled(false);
  }

  CreateCannonball();

  // 캐논볼 알까기
  function ShootCannonball() {
    const clone = cannonball.clone("clone");
    clone.position = camera.position;

    clone.setEnabled(true);

    clone.physicsImpostor.applyForce(
      // 방향, 포지션
      camera.getForwardRay().direction,
      clone.getAbsolutePosition()
    );

    clone.physicsImpostor.registerOnPhysicsCollide(
      ground.physicsImpostor,
      () => {
        setTimeout(() => {
          clone.dispose(); //폐기
        }, 3000);
      }
    );
  }

  //캐논볼 발사
  scene.onPointerDown = (e) => {
    if (e.button === 2) ShootCannonball();
  };

  return scene;
}
```


Raycasting 
----------

![image](https://user-images.githubusercontent.com/30430227/174721267-f57dfc78-fd77-4364-b417-720b987ba6ed.png)

```
const canvas = document.querySelector("#renderCanvas");
const engine = new BABYLON.Engine(canvas, true);

// 비동기 버전
function startRenderLoop(sceneToRender) {
  engine.runRenderLoop(() => {
    sceneToRender.render();
  });
}

createScene().then(startRenderLoop);

async function createScene() {
  const scene = new BABYLON.Scene(engine);

  const camera = new BABYLON.FreeCamera(
    "camera",
    new BABYLON.Vector3(0, 1, -5),
    scene
  );
  camera.attachControl(canvas, false);
  // camera.applyGravity = true;
  // camera.checkCollisions = true;
  // camera.ellipsoid = new BABYLON.Vector3(1, 1, 1);
  camera.minZ = 0.1;
  camera.speed = 0.75;
  camera.angularSensibility = 4000; //카메라 회전 감도, 낮을 수록 높다

  camera.setTarget(BABYLON.Vector3.Zero());

  //라이트
  new BABYLON.HemisphericLight("hemi", new BABYLON.Vector3(0, 1, 0), scene);

  const framesPerSecond = 60;
  const gravity = -9.81;
  scene.gravity = new BABYLON.Vector3(0, gravity / framesPerSecond, 0);
  scene.collisionsEnabled = true;

  //재질
  function randColor() {
    randMat = new BABYLON.PBRMaterial("randMat", scene);
    randMat.albedoColor = new BABYLON.Color3(
      Math.random(),
      Math.random(),
      Math.random()
    );
    randMat.roughness = 1;

    return randMat;
  }

  // 비동기 버전 GLB
  const { meshes: meshA } = await BABYLON.SceneLoader.ImportMeshAsync(
    //{}구조분해할당 별명 :meshA
    "",
    "./gltfs/",
    "fpc.glb",
    scene
  );

  meshA.map((mesh) => {
    mesh.checkCollisions = true;
    mesh.material = randColor();
  });

  scene.enablePhysics(
    new BABYLON.Vector3(0, -9.81, 0),
    new BABYLON.CannonJSPlugin()
  );

  //바닥
  const ground = BABYLON.MeshBuilder.CreateGround("ground", {
    width: 40,
    height: 40,
  });
  ground.position.y = -1;
  ground.isVisible = false; //랜더 시 안보이게

  ground.physicsImpostor = new BABYLON.PhysicsImpostor(
    ground,
    BABYLON.PhysicsImpostor.BoxImpostor,
    { mass: 0, resitution: 0.9, friction: 10 }, //mass 0 - 고정
    scene
  );

  // 지구
  const earth = BABYLON.MeshBuilder.CreateSphere("earth", { diameter: 3 });
  earth.position.y = 3;
  const earthMat = new BABYLON.PBRMaterial("earthMat", scene);
  earthMat.albedoColor = new BABYLON.Color3(1, 1, 1);
  earthMat.roughness = 1;

  earth.material = earthMat;

  earth.physicsImpostor = new BABYLON.PhysicsImpostor(
    earth,
    BABYLON.PhysicsImpostor.SphereImpostor,
    { mass: 1, friction: 1 }
  );

  //페인팅 재질
  let splatters;

  function CreateTextures() {
    const blue = new BABYLON.PBRMaterial("blue", scene);
    const orange = new BABYLON.PBRMaterial("orange", scene);
    const green = new BABYLON.PBRMaterial("green", scene);

    blue.albedoTexture = new BABYLON.Texture("./images/blue.png", scene);
    orange.albedoTexture = new BABYLON.Texture("./images/orange.png", scene);
    green.albedoTexture = new BABYLON.Texture("./images/green.png", scene);

    blue.roughness = 1;
    orange.roughness = 1;
    green.roughness = 1;

    blue.albedoTexture.hasAlpha = true;
    orange.albedoTexture.hasAlpha = true;
    green.albedoTexture.hasAlpha = true;

    blue.zOffset = -0.25; //재질이 표면에 매핑되는 위치, 음수-표면 바깥에 위치
    orange.zOffset = -0.25;
    green.zOffset = -0.25;

    splatters = [blue, orange, green];
  }

  CreateTextures();

  //레이캐스트
  function CreatePickingRay() {
    scene.onPointerDown = () => {
      const ray = scene.createPickingRay(
        scene.pointerX,
        scene.pointerY,
        BABYLON.Matrix.Identity(), //피킹 대상,identity(정체성,단위) - 행렬이 단위 행렬인지 체크
        camera
      );

      const raycastHit = scene.pickWithRay(ray);

      //raycastHit - 화면 클릭 시 발생, raycastHit.hit -Mesh 클릭했을 때 발생
      //if(raycastHit){
      // console.log("니 주글래!");
      // }
      if (raycastHit.hit && raycastHit.pickedMesh.name === "earth") {
        const decal = BABYLON.MeshBuilder.CreateDecal(
          "decal",
          raycastHit.pickedMesh,
          {
            position: raycastHit.pickedPoint,
            normal: raycastHit.getNormal(true),
            size: new BABYLON.Vector3(1, 1, 1),
          }
        );
        decal.material =
          splatters[Math.floor(Math.random() * splatters.length)];

        decal.setParent(raycastHit.pickedMesh); //데칼을 지구에 붙인다

        //충격파
        raycastHit.pickedMesh.physicsImpostor.applyImpulse(
          ray.direction, //ray.direction.scale(5) 더 강하게
          raycastHit.pickedPoint
        );
      }
    };
  }

  CreatePickingRay();

  return scene;
}
```


기본 애니메이션 M-마우스 클릭 시 사라짐
------------------------------------

![image](https://user-images.githubusercontent.com/30430227/174770168-0c44aae4-d275-465e-a2ea-21ef32b4ccc0.png)

```
const canvas = document.querySelector("#renderCanvas");
const engine = new BABYLON.Engine(canvas, true);

// 비동기 버전
function startRenderLoop(sceneToRender) {
  engine.runRenderLoop(() => {
    sceneToRender.render();
  });
}

createScene().then(startRenderLoop);

async function createScene() {
  const scene = new BABYLON.Scene(engine);

  const camera = new BABYLON.FreeCamera(
    "camera",
    new BABYLON.Vector3(0, 1, -5),
    scene
  );
  camera.attachControl(canvas, false);
  // camera.applyGravity = true;
  // camera.checkCollisions = true;
  // camera.ellipsoid = new BABYLON.Vector3(1, 1, 1);
  camera.minZ = 0.1;
  camera.speed = 0.75;
  camera.angularSensibility = 4000; //카메라 회전 감도, 낮을 수록 높다

  camera.setTarget(BABYLON.Vector3.Zero());

  //라이트
  new BABYLON.HemisphericLight("hemi", new BABYLON.Vector3(0, 1, 0), scene);

  const framesPerSecond = 60;
  const gravity = -9.81;
  scene.gravity = new BABYLON.Vector3(0, gravity / framesPerSecond, 0);
  scene.collisionsEnabled = true;

  //재질
  function randColor() {
    randMat = new BABYLON.PBRMaterial("randMat", scene);
    randMat.albedoColor = new BABYLON.Color3(
      Math.random(),
      Math.random(),
      Math.random()
    );
    randMat.roughness = 1;

    return randMat;
  }

  // 비동기 버전 GLB
  const { meshes: meshA } = await BABYLON.SceneLoader.ImportMeshAsync(
    //{}구조분해할당 별명 :meshA
    "",
    "./gltfs/",
    "fpc.glb",
    scene
  );

  meshA.map((mesh) => {
    mesh.checkCollisions = true;
    mesh.material = randColor();
  });

  const { meshes: meshB } = await BABYLON.SceneLoader.ImportMeshAsync(
    "",
    "./gltfs/",
    "suzi.glb",
    scene
  );

  meshB.shift(); //메쉬 리스트 쉬프트 1 -> 0

  // meshB[1].rotationQuaternion = null; //gltf는 Quaternion으로 export한다
  meshB[0].rotationQuaternion = null; //gltf는 Quaternion으로 export한다
  // meshB[0].visibility = 0.5;

  // const target = meshB[1];
  const target = meshB[0];

  scene.enablePhysics(
    new BABYLON.Vector3(0, -9.81, 0),
    new BABYLON.CannonJSPlugin()
  );

  //바닥
  const ground = BABYLON.MeshBuilder.CreateGround("ground", {
    width: 40,
    height: 40,
  });
  ground.position.y = -1;
  ground.isVisible = false; //랜더 시 안보이게

  ground.physicsImpostor = new BABYLON.PhysicsImpostor(
    ground,
    BABYLON.PhysicsImpostor.BoxImpostor,
    { mass: 0, resitution: 0.9, friction: 100 }, //mass 0 - 고정
    scene
  );

  function CreateAnimations() {
    const rotateFrames = []; //keyframe 저장 배열
    const slideFrames = [];
    const fadeFrames = [];

    const fps = 60;

    rotateFrames.push({ frame: 0, value: 0 });
    rotateFrames.push({ frame: 180, value: Math.PI * 2 });

    slideFrames.push({ frame: 0, value: new BABYLON.Vector3(0, 0, 0) });
    slideFrames.push({ frame: 60, value: new BABYLON.Vector3(-1, 2, 0) });
    slideFrames.push({ frame: 120, value: new BABYLON.Vector3(2, 1, 0) });
    slideFrames.push({ frame: 180, value: new BABYLON.Vector3(0, 0, 0) });

    fadeFrames.push({ frame: 0, value: 1 });
    fadeFrames.push({ frame: 180, value: 0 });

    const rotateAnim = new BABYLON.Animation(
      "rotateAnim",
      "rotation.y",
      fps,
      BABYLON.Animation.ANIMATIONTYPE_FLOAT,
      // BABYLON.Animation.ANIMATIONLOOPMODE_CONSTANT
      BABYLON.Animation.ANIMATIONLOOPMODE_CYCLE
    );

    const slideAnim = new BABYLON.Animation(
      "slideAnim",
      "position",
      fps,
      BABYLON.Animation.ANIMATIONTYPE_VECTOR3,
      BABYLON.Animation.ANIMATIONLOOPMODE_CYCLE
    );

    const fadeAnim = new BABYLON.Animation(
      "fadeAnim",
      "visibility",
      fps,
      BABYLON.Animation.ANIMATIONTYPE_FLOAT,
      BABYLON.Animation.ANIMATIONLOOPMODE_CONSTANT
    );

    rotateAnim.setKeys(rotateFrames);
    slideAnim.setKeys(slideFrames);
    fadeAnim.setKeys(fadeFrames);

    target.animations.push(rotateAnim);
    target.animations.push(slideAnim);
    target.animations.push(fadeAnim);

    // 1. 모든 애니메이션 실행
    // scene.beginAnimation(target, 0, 180, true); //true =loop

    // 2. 선택 애니메이션 실행 directAnimation
    // scene.beginDirectAnimation(target, [slideAnim, rotateAnim], 0, 180, true);

    // scene.onPointerDown = (evt) => {
    //   if (evt.button === 1)
    //     scene.beginDirectAnimation(target, [fadeAnim], 0, 180, true);
    // };

    // 3.종료 애니메이션
    function onAnimationEnd() {
      console.log("니주글래!");
      target.setEnabled(false); //메시를 랜더링 하지 않음
    }

    const animControl = scene.beginDirectAnimation(
      target,
      [slideAnim, rotateAnim],
      0,
      180,
      true, //loop-true
      1, //speed Ratio
      onAnimationEnd
    );

    scene.onPointerDown = async (evt) => {
      if (evt.button === 1) {
        await scene
          .beginDirectAnimation(target, [fadeAnim], 0, 180) //loop-false
          .waitAsync(); // 애니메이션(fadeAnim) 이 끝날 때까지 기다림, 프로미스
        animControl.stop();
      }
    };
  }

  CreateAnimations();

  // scene.debugLayer.show();

  return scene;
}
```


캐릭터 애니메이션
------------------

[무료게임어셋](https://www.kenney.nl/assets/animated-characters-2)

![image](https://user-images.githubusercontent.com/30430227/174774965-8ffd8b0a-95b9-4504-bbe3-1a728b7e0901.png)

1. Mixamo 

`idle, jump, run(inplace) 다운`

![image](https://user-images.githubusercontent.com/30430227/174917668-183a157c-4492-4873-a450-3e7be608472d.png)


2. Blender

`Mesh Import > Shader Editro > Skin Texture > Emission:0, Alpha: 1`

![image](https://user-images.githubusercontent.com/30430227/174918719-1b5d781e-4099-419b-8093-5c7ff2fcb1c2.png)
![image](https://user-images.githubusercontent.com/30430227/174918690-001b131c-5b91-42f8-a7fa-26a3cc5170f4.png)

`Animation FBX Import > Action Editor(Dopesheet) > Rename > Root.001 삭제(애니메이션은 남아있다)`

![image](https://user-images.githubusercontent.com/30430227/174919299-625f58d7-a40e-4892-861f-636b1e872141.png)

![image](https://user-images.githubusercontent.com/30430227/174919263-31ea1741-5598-4a7c-abff-3694f7310d1a.png)
![image](https://user-images.githubusercontent.com/30430227/174919509-d75686c7-dd72-4440-8922-3ba43c732173.png)

`Root 선택 > ActoinEditor > Animation Clip 선택`

![image](https://user-images.githubusercontent.com/30430227/174920094-89e0ed1a-f9ea-4e1f-9da7-af409280b30f.png)
![image](https://user-images.githubusercontent.com/30430227/174920079-433f0c61-272f-46e1-9803-a792a91f5a20.png)

`NonlinearEditor > 'Root' 트랙 선택 > Add Action Strip > Rename`

![image](https://user-images.githubusercontent.com/30430227/174925539-c9e43d5c-259b-4b45-abb6-a94980d94a82.png)

![image](https://user-images.githubusercontent.com/30430227/174925608-92733fd8-734d-4d4c-b91c-91e8a4f21f2c.png)
![image](https://user-images.githubusercontent.com/30430227/174925633-a76fe326-99da-4121-b95b-b865317a1d9f.png)

`별 버튼-Play Solo`

![image](https://user-images.githubusercontent.com/30430227/174926732-4776d3e5-a21f-4c94-a383-b39e4946304f.png)

`Export GLB - Compression`

![image](https://user-images.githubusercontent.com/30430227/174927028-43b6a925-3e6e-48b6-bd16-9ca07c5fb7b0.png)


3. BabylonJS

![image](https://user-images.githubusercontent.com/30430227/174961550-8ebeae0d-bc72-425b-97a7-4750ec17fe92.png)

```
const canvas = document.querySelector("#renderCanvas");
const engine = new BABYLON.Engine(canvas, true);

// 비동기 버전
function startRenderLoop(sceneToRender) {
  engine.runRenderLoop(() => {
    sceneToRender.render();
  });
}

createScene().then(startRenderLoop);

async function createScene() {
  const scene = new BABYLON.Scene(engine);

  const camera = new BABYLON.FreeCamera(
    "camera",
    new BABYLON.Vector3(0, 3, -5),
    scene
  );
  // camera.attachControl(canvas, false);
  camera.minZ = 0.1;
  camera.speed = 0.75;
  camera.angularSensibility = 4000; //카메라 회전 감도, 낮을 수록 높다

  camera.setTarget(new BABYLON.Vector3(0, 2, 0));

  //라이트
  new BABYLON.HemisphericLight("hemi", new BABYLON.Vector3(0, 1, 0), scene);

  const framesPerSecond = 60;
  // const gravity = -9.81;
  // scene.gravity = new BABYLON.Vector3(0, gravity / framesPerSecond, 0);
  // scene.collisionsEnabled = true;

  //재질
  function randColor() {
    randMat = new BABYLON.PBRMaterial("randMat", scene);
    randMat.albedoColor = new BABYLON.Color3(
      Math.random(),
      Math.random(),
      Math.random()
    );
    randMat.roughness = 1;

    return randMat;
  }

  // 비동기 버전 GLB
  const { meshes: meshA } = await BABYLON.SceneLoader.ImportMeshAsync(
    //{}구조분해할당 별명 :meshA
    "",
    "./gltfs/",
    "fpc.glb",
    scene
  );

  meshA.map((mesh) => {
    // mesh.checkCollisions = true;
    // mesh.material = randColor();
    mesh.position.y = 0.6;
  });

  const { meshes: meshB, animationGroups } =
    await BABYLON.SceneLoader.ImportMeshAsync(
      "",
      "./gltfs/",
      "character.glb",
      scene
    );

  meshB[0].rotate(BABYLON.Vector3.Up(), -Math.PI / 2); //메쉬 회전(축,값)
  meshB[0].position = new BABYLON.Vector3(3, 0, 0);

  animationGroups[0].stop();
  animationGroups[2].play(true);

  const run = animationGroups[2];

  // 어텍 애니메이션
  let zombieAnims;
  CreateZombie();

  const attackAnim = animationGroups[1].targetedAnimations[0].animation; //animationGroup[1]이 실행되면 타킷 애니

  // 어텍 이벤트 60 frame 후에 실행, loop=false
  const attackEvt = new BABYLON.AnimationEvent(
    60,
    () => {
      zombieAnims[2].stop();
      zombieAnims[1].play();
    },
    false
  );

  attackAnim.addEvent(attackEvt);

  scene.onPointerDown = (evt) => {
    if (evt.button === 2) animationGroups[1].play();
  };

  //쌍동이 BLB 불러옴
  async function CreateZombie() {
    const { meshes, animationGroups } =
      await BABYLON.SceneLoader.ImportMeshAsync(
        "",
        "./gltfs/",
        "character.glb",
        scene
      );

    zombieAnims = animationGroups;

    meshes[0].rotate(BABYLON.Vector3.Up(), Math.PI / 2);
    meshes[0].position = new BABYLON.Vector3(-3, 0, 0);

    zombieAnims[0].stop();
    animationGroups[2].play(true);

    const deathAnim = animationGroups[1].targetedAnimations[0].animation;

    const deathEvt = new BABYLON.AnimationEvent(
      80,
      () => {
        run.play(true); //좀비 죽으면 달리기
      },
      false
    );
    deathAnim.addEvent(deathEvt);
  }

  //커스텀 씬 애니메이션 추가 - 카메라 Animation
  async function CreateCstmscene() {
    const camKeys = [];
    const fps = 60;
    const camAnim = new BABYLON.Animation(
      "camAnim",
      "position",
      fps,
      BABYLON.Animation.ANIMATIONTYPE_VECTOR3,
      BABYLON.Animation.ANIMATIONLOOPMODE_CONSTANT,
      true
    );

    camKeys.push({ frame: 0, value: new BABYLON.Vector3(0, 3, -5) });
    camKeys.push({ frame: 3 * fps, value: new BABYLON.Vector3(-8, 10, -20) }); //좌로8, 위로10, 뒤로20
    camKeys.push({ frame: 6 * fps, value: new BABYLON.Vector3(8, 10, -20) });
    camKeys.push({ frame: 9 * fps, value: new BABYLON.Vector3(0, 3, -5) });

    camAnim.setKeys(camKeys);

    camera.animations.push(camAnim);

    await scene.beginAnimation(camera, 0, 12 * fps).waitAsync(); //애니메이션 끝날 때까지 기다려..
    EndCstmscene();
  }

  CreateCstmscene();

  function EndCstmscene() {
    camera.attachControl(scene, false);
    animationGroups[2].stop();
    animationGroups[0].play();
  }

  //바닥
  const ground = BABYLON.MeshBuilder.CreateGround("ground", {
    width: 40,
    height: 40,
  });
  ground.position.y = -1;
  ground.isVisible = false; //랜더 시 안보이게

  // ground.physicsImpostor = new BABYLON.PhysicsImpostor(
  //   ground,
  //   BABYLON.PhysicsImpostor.BoxImpostor,
  //   { mass: 0, resitution: 0.9, friction: 100 }, //mass 0 - 고정
  //   scene
  // );

  // scene.debugLayer.show();

  return scene;
}
```
