<template>
  <div id="HelloWorld">
  </div>
</template>

<script>
import * as THREE from "three";
import { OrbitControls } from "three/examples/jsm/controls/OrbitControls";
import worldAsset from "../assets/world.jpg";

export default {
  name: 'HelloWorld',
  data() {
    return {
    }
  },
  methods: {
    initialize() {
      this.container = this.$el;
      
      this.createScene();
      this.createCamera();
      this.createRenderer();
      this.createControls();
      this.createMesh();
      this.animate();
    },
    createScene() {
      // Initialize scene
      this.scene = new THREE.Scene();
    },
    createCamera() {
      // Initialize camera
      this.camera = new THREE.PerspectiveCamera(50, window.innerWidth / window.innerHeight, 0.1);

      // Reposition camera
      this.camera.position.set(5, 0, 0);
    },
    createRenderer() {
      // Initialize renderer
      this.renderer = new THREE.WebGLRenderer({
        alpha: true,
        antialias: true
      });

      // Set renderer size
      this.renderer.setSize(window.innerWidth, window.innerHeight);

      // Append renderer to body
      this.container.appendChild(this.renderer.domElement);
    },
    createControls() {
      this.controls = new OrbitControls(this.camera, this.container);
      this.controls.enableDamping = true;
      this.controls.campingFactor = 0.25;
      this.controls.enableZoom = true;
    },
    createMesh() {
      // Load world texture
      const worldTexture = new THREE.TextureLoader().load(worldAsset);

      // Initialize world geometry
      const worldGeometry = new THREE.SphereGeometry(1, 40, 40);

      // Initialize world material
      const worldMaterial = new THREE.MeshBasicMaterial({ map: worldTexture });

      // Initialize world
      this.mesh = new THREE.Mesh(worldGeometry, worldMaterial);

      // Add earth to scene
      this.scene.add(this.mesh);
    },
    updateRendererSize() {
      // Update camera aspect
      this.camera.aspect = window.innerWidth / window.innerHeight;

      // Update camera projection matrix
      this.camera.updateProjectionMatrix();

      // Resize renderer
      this.renderer.setSize(window.innerWidth, window.innerHeight);
    },
    createOnWindowResize() {
      window.addEventListener("resize", () => this.updateRendererSize());
    },
    removeOnWindowResize() {
      // Resize
      window.removeEventListener("resize", () => this.updateRendererSize());
    },
    animate() {
      // Request animation frame
      window.requestAnimationFrame(this.animate);

      this.updateRendererSize();
      
      // Rotate world
      this.mesh.rotation.y += 0.0010;

      // Render scene
      this.renderer.render(this.scene, this.camera);
    }
  },
  created() {
    this.createOnWindowResize();
  },
  unmounted() {
    this.removeOnWindowResize();
  },
  mounted() {
    this.initialize();
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
