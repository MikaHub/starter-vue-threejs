<template>
  <div id="canvas"></div>
</template>

<script>
import * as Three from 'three'
import {toRaw} from "vue";
import { GLTFLoader } from 'three/examples/jsm/loaders/GLTFLoader';


export default {
  name: 'ThreeComponent',
  mounted() {
    window.addEventListener('resize', () => {
      this.windowHeight = window.innerHeight
      this.windowWidth = window.innerWidth

      this.camera.aspect =  this.windowWidth / this.windowHeight
      this.camera.updateProjectionMatrix()

      this.renderer.setSize(this.windowWidth, this.windowHeight)
      this.renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2))
    })

    this.init();
    this.animate();
  },
  data() {
    return {
      camera: null,
      scene: null,
      renderer: null,
      mesh: null,
      windowHeight: window.innerHeight,
      windowWidth: window.innerWidth,
    }
  },
  methods: {
    init: function() {

      const canvas = document.getElementById("canvas")

      this.camera = new Three.PerspectiveCamera(75, this.windowWidth/this.windowHeight, 0.01, 10);
      this.camera.position.z = 1;

      const gltfLoader = new GLTFLoader()

      this.scene = new Three.Scene();

      gltfLoader.load(require('../assets/phone.gltf'), (gltf) => {
        console.log(gltf)
        this.scene.add(gltf.scene)
      })

      this.renderer = new Three.WebGLRenderer({antialias: true});
      this.renderer.setSize(this.windowWidth, this.windowHeight);
      this.renderer.setClearColor( 0xffffff, 0);
      canvas.appendChild(this.renderer.domElement);

    },
    animate: function() {
      requestAnimationFrame(this.animate);
      // this.mesh.rotation.x += 0.01;
      // this.mesh.rotation.y += 0.02;
      this.renderer.render(toRaw(this.scene), this.camera);
    }
  },
}
</script>

<style scoped>

</style>