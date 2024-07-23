<template>
    <div ref="rendererContainer"></div>
</template>
  
  <script>
  import { onMounted, ref } from 'vue';
  import * as THREE from 'three';
  import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls';
  
  export default {
    setup() {
      const rendererContainer = ref(null);
  
      onMounted(() => {
        const scene = new THREE.Scene();
        const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
        const renderer = new THREE.WebGLRenderer();
        renderer.setSize(window.innerWidth, window.innerHeight);
        rendererContainer.value.appendChild(renderer.domElement);
  
        const controls = new OrbitControls(camera, renderer.domElement);
        camera.position.set(0, 0, 3);
        controls.update();
  
        const geometry = new THREE.SphereGeometry(0.5, 32, 32);
        const material = new THREE.MeshBasicMaterial({ color: 0xcccccc });
        const sphere = new THREE.Mesh(geometry, material);
        scene.add(sphere);
  
        const animate = function () {
          requestAnimationFrame(animate);
          controls.update();
          renderer.render(scene, camera);
        };
  
        animate();
      });
  
      return { rendererContainer };
    }
  };
  </script>
  
  <style scoped>
  div {
    width: 100%;
    height: 100vh;
  }
  </style>
  