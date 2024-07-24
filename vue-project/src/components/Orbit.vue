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
        camera.position.set(0, 0, 10);
        controls.update();
  
        // Function to create a sphere
        const createSphere = (radius, color, position) => {
          const geometry = new THREE.SphereGeometry(radius, 32, 32);
          const material = new THREE.MeshBasicMaterial({ color });
          const sphere = new THREE.Mesh(geometry, material);
          sphere.position.set(...position);
          scene.add(sphere);
          return sphere;
        };
  
        // Function to create an elliptical orbit line
        const createOrbit = (radiusX, radiusY, segments, color) => {
          const curve = new THREE.EllipseCurve(
            0, 0,            // ax, aY
            radiusX, radiusY, // xRadius, yRadius
            0, 2 * Math.PI,  // startAngle, endAngle
            false,           // clockwise
            0                // rotation
          );
  
          const points = curve.getPoints(segments);
          const geometry = new THREE.BufferGeometry().setFromPoints(points);
          const material = new THREE.LineBasicMaterial({ color });
          const orbit = new THREE.Line(geometry, material);
          orbit.rotation.x = Math.PI / 2; // Rotate to make it horizontal
          scene.add(orbit);
          return orbit;
        };
  
        // Create multiple spheres and their elliptical orbits
        const spheres = [
          createSphere(0.3, 0x0077ff, [2, 0, 0]),
          createSphere(0.3, 0xff7700, [-2, 0, 0]),
          createSphere(0.3, 0x77ff00, [0, 2, 0]),
          createSphere(0.3, 0xff0077, [0, -2, 0])
        ];
  
        const orbits = [
          createOrbit(2, 1.5, 64, 0xffffff), // White elliptical orbit
          createOrbit(2.5, 2, 64, 0xffffff), // White elliptical orbit
          createOrbit(3, 2.5, 64, 0xffffff), // White elliptical orbit
          createOrbit(3.5, 3, 64, 0xffffff)  // White elliptical orbit
        ];
  
        // Animation loop
        const animate = function () {
          requestAnimationFrame(animate);
  
          // Rotate spheres around the center
          spheres.forEach((sphere, index) => {
            const angle = Date.now() * 0.001 + index;
            sphere.position.x = Math.cos(angle) * (index + 2);
            sphere.position.y = Math.sin(angle) * (index + 2);
          });
  
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
  