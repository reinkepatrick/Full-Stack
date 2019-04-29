# WebGL

WebGL ist eine 3D Grafik API basierend auf OpenGL (genauer, OpenGL ES, für Embedded Systems). Die API ist für Verwendung in Javascript/ECMAScript in HTML5 gedacht und somit für alle Platformen die HTML5 unterstützen verfügbar.

Mit WebGL gerenderte Elemente werden im HTML Canvas Element dargestellt, mit Hilfe eines eigens definierten RenderingContext, **WebGLRenderingContext**, welcher den standardmäßigen CanvasRenderingContext2D ersetzt.

Die API ist low-level OpenGL, und enthält nahezu keinen boilerplate code, d.h. direktes Arbeiten mit Vertices und Framebuffern. Der Grund dafür ist, dass die API möglichst simpel und flexibel bleiben soll. Es existieren jedoch mehrere Bibliotheken die auf bestimmte use-cases zugeschnitten sind.

## Relevante Bibliotheken

|Name|WebGL Support| Basis Features | Zusatzfunktionen | Plugins | Fazit |
|----|-------------|----------------|------------------|---------|-------|
| three.js | WebGL 1.0, begrenzt 2.0 | Licht/Schatten, Kamera, Material, Animation | Natives laden von .obj, .mtl, .gltf, .svg | Diverse, z.B. Physijs als Physics-Engine | Grundsätzliche Funktionen, aber trotzdem noch sehr nah an OpenGL, wenig modern|
| BabylonJS | WebGL 2.0 | Licht/Schatten, Kamera, Material, Animation, Shader | Mesh + Morph, Physics- und Collision-Engine, Partikel System, Audio-Engine, FX Toolkit (Fog, Shadow Maps, Anti-Aliasing, Glow) | | Deutlich mehr Funktionen (dadurch aber auch mehr bloat), größere Community, volles Toolkit für Spiel-Design|
| D3.js | - | Datenverarbeitung, SVG und DOM-Manipulation, Dynamische Änderung & Animation (geringe Performanz) | | | Sinnvoll als teil eines Hybriden ansatzes, z.B. zum Zeichnen von statischen Elementen (Koordinaten System)|
| StardustJS | Unbekannt, aber für Anwendung irrelevant | Datenvisualisierung, Databinding, 2D & 3D | | | Einzig für (animierte) Datenvisualisierung, gut für große Datenmengen, mehr Potential als SVG Darstellung |


## 3D WebGL mit BabylonJS

Babylon ist primär für 3D Darstellung gedacht und ein Großteil der Features fällt in die Kategorie Gamedesign. Funktionen wie eine Integriere Physicsengine, FX und Partikel sind hierfür durchaus hilfreich. Die grundlegenden Funktionen in Babylon sind jedoch auch diverse andere Anwendungsfälle nützlich.
In seiner Struktur und im Programmier-Ansatz ist die Bibliothek offensichtlich Objektorientiert aufgebaut. Die Szene, so wie Licht, Kamera und alle Meshes, werden als Objekte ihrer jeweiligen Klasen instantiiert und positioniert. Meshes bekommen Materialien zugewiesen, Kameras und Lichtquellen haben diverse manipulierbare Attribute. Auch die Physicsengine funktioniert mit sogenannten „Imposter“ Klassen, die ihre jeweiligen Verhalten bestimmen. Damit ist Babylon leicht zu strukturieren und in Komponenten zu teilen und erlaubt eine sehr hohe Konfigurierbarkeit.
```javascript
function createScene() {	
	//Szene
	var scene = new BABYLON.Scene(engine);

	//Licht
	var light = new BABYLON.HemisphericLight("light1", new BABYLON.Vector3(1, 1, 0), scene);

	//Kamera
	var camera = new BABYLON.UniversalCamera("UniversalCamera", new BABYLON.Vector3(0, 0, -10), scene);
	//Kamera Steuerung
	camera.attachControl(canvas, true);

	//Kugel
	var sphere = BABYLON.MeshBuilder.CreateSphere("sphere", {diameter:1}, scene);

	return scene;
}
```
*Aufbau einer einfachen Szene mit Licht, Kamera und Kugel Mesh*

Meshes können in diversen Formaten importiert werden (wie z.B. glTF oder obj), jedoch biete die Bibliothek auch Hilfsmethoden an, mit denen alle gängigen euklidischen Körper (Würfel, Kugel, Zylinder, Kegel, Torus) generiert werden können. 

![Babylon Meshes](./img/babylon_objects.jpg)
<p align="justify">*Eine Auswahl von Meshes die mit Babylon generiert werden können*</p>

