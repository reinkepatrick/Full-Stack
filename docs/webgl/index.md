# WebGL

WebGL ist eine 3D Grafik API basierend auf OpenGL (genauer, OpenGL ES, für Embedded Systems). Die API ist für Verwendung in Javascript/ECMAScript in HTML5 gedacht und somit für alle Platformen die HTML5 unterstützen verfügbar.

Mit WebGL gerenderte Elemente werden im HTML Canvas Element dargestellt, mit Hilfe eines eigens definierten RenderingContext, **WebGLRenderingContext**, welcher den standardmäßigen CanvasRenderingContext2D ersetzt.

Die API ist low-level OpenGL, und enthält nahezu keinen boilerplate code, d.h. direktes Arbeiten mit Vertices und Framebuffern. Der Grund dafür ist, dass die API möglichst simpel und flexibel bleiben soll. Es existieren jedoch mehrere Bibliotheken die auf bestimmte use-cases zugeschnitten sind.

##WebGL Bibliotheken

### three.js

- Support für WebGL 1.0, begrenzter Support für WebGL 2.0
- Licht/Schatten, Kamera, Material, Animation
- Natives laden von .obj, .mtl, .gltf, .svg
- Diverse plugins wie z.B. Physijs als Physics-Engine
- **Fazit:** Grundsätzliche Funktionen, aber trotzdem noch sehr nah an OpenGL, wenig modern

### BabylonJS

- Vollständiger WebGL 2.0 Support
- Licht/Schatten, Kamera, Material, Animation
- Mesh + Morph Funktionen
- Integrierte Physics- und Collision-Engine
- Partikel System
- Integrierte Audio-Engine
- Voller Shader Support
- FX Toolkit (Fog, Shadow Maps, Anti-Aliasing, Glow)
- **Fazit:** Deutlich mehr Funktionen (dadurch aber auch mehr bloat), größere Community, volles Toolkit für Spiel-Design

### D3.js

- Data-Driven Documents, gedacht für Datenvisualisierung
- Support für so ziemlich alle komplexe Graphen und Chart Darstellungen
- Dynamisches Rendering, animierte Darstellungen
- **Fazit:** Für Diagramme super, aber auch nur dafür gedacht.



## WebGL Setup für BabylonJS

1. Neues Projekt angelegen, mit `index.html` und `js/index.js`

2. Scripte Einfügen:

   ``````html
   <!-- Cannon Physics:-->
   <script src="https://cdn.babylonjs.com/cannon.js"></script>
   <!-- Babylon Core:-->
   <script src="https://preview.babylonjs.com/babylon.js"></script>
   <!-- Babylon Loaders:-->
   <script src="https://preview.babylonjs.com/loaders/babylonjs.loaders.min.js"></script>
   <!-- jquery Polyfill:-->
   <script src="https://code.jquery.com/pep/0.4.3/pep.js"></script>
   ``````

3. HTML Canvas Zufügen und index.js einbinden:

   ```html
   <canvas id="renderCanvas" touch-action="none"></canvas>
   <script src="./js/index.js"></script>
   ```

4. In index.js die Basics für eine Szene etablieren:

   ```js
   var canvas = document.getElementById("renderCanvas"); // Get the canvas element 
   
   var engine = new BABYLON.Engine(canvas, true); // Generate the BABYLON 3D engine
   
   /******* Add the create scene function ******/
   var createScene = function () {
   	
   	// Create the scene space
   	var scene = new BABYLON.Scene(engine);
   	
   	// Add a camera to the scene and attach it to the canvas
   	var camera = new BABYLON.ArcRotateCamera("Camera", Math.PI / 2, Math.PI / 2, 2, new BABYLON.Vector3(0,10,10), scene);
       camera.setTarget(BABYLON.Vector3.Zero());
   	camera.attachControl(canvas, true);
   	
   	// Add lights to the scene
   	var light1 = new BABYLON.HemisphericLight("light1", new BABYLON.Vector3(1, 1, 0), scene);
   	var light2 = new BABYLON.PointLight("light2", new BABYLON.Vector3(0, 1, -1), scene);
   	
   	var ground = BABYLON.Mesh.CreateGround("ground1", 24, 24, 2, scene);
     	
   	// Add and manipulate meshes in the scene
   	var sphere = BABYLON.MeshBuilder.CreateSphere("sphere", {diameter:1, updatable: true}, scene);
   	sphere.position.y = 1;
   	return scene;
   };
   /******* End of the create scene function ******/    
   
   var scene = createScene(); //Call the createScene function
   
   // Register a render loop to repeatedly render the scene
   engine.runRenderLoop(function () { 
   	scene.render();
   });
   
   // Watch for browser/canvas resize events
   window.addEventListener("resize", function () { 
   	engine.resize();
   });
   
   ```

5. Fertig! Ab hier kann dann mit anderen Meshes oder Physics experimentiert werden