<template>
  <div class="earth"></div>
</template>

<script>
import TextureMap from '@/assets/images/earthmap.jpg';
import TextureCloud from '@/assets/images/earthcloudmap.jpg';
import TextureSpec from '@/assets/images/earthspec.jpg';
import * as THREE from 'three';

const textureLoader = new THREE.TextureLoader();
const loadTexture = path => textureLoader.load(
  path,
  (tx => new THREE.MeshBasicMaterial({
    map: tx,
    refractionRatio: 1,
  })),
  undefined,
  (err => console.error(err)),
);
const textureMap = loadTexture(TextureMap);
const textureMapCloud = loadTexture(TextureCloud);
const textureSpec = loadTexture(TextureSpec);

const createMeshEarth = () => {
  const geometry = new THREE.SphereGeometry(0.56, 32, 32);
  const material = new THREE.MeshPhongMaterial({
    map: textureMap,
    opacity: 0.7,
    specularMap: textureSpec,
    specular: new THREE.Color('grey'),
  });

  return new THREE.Mesh(geometry, material);
};

const createMeshEarthCloud = () => {
  const geometry = new THREE.SphereGeometry(0.566, 32, 32);
  const material = new THREE.MeshPhongMaterial({
    map: textureMapCloud,
    side: THREE.DoubleSide,
    transparent: true,
    depthWrite: false,
    opacity: 0.1,
  });

  return new THREE.Mesh(geometry, material);
};

export default {
  mounted() {
    const onRenderLoop = [];
    const camera = new THREE.PerspectiveCamera(
      70,
      window.innerWidth / window.innerHeight,
      0.01,
      20,
    );
    camera.position.z = 1;

    const scene = new THREE.Scene();

    const lightAmbient = new THREE.AmbientLight(0x222222, 7);
    scene.add(lightAmbient);

    const light = new THREE.SpotLight(0xffffff, 0.3);
    light.position.set(-3, 10, -3);
    light.castShadow = true;
    scene.add(light);

    const lightDark = new THREE.SpotLight(0xffffff, 1);
    lightDark.position.set(5, -10, 2);
    lightDark.distance = -10;
    lightDark.power = -10;
    lightDark.decay = -0.1;
    lightDark.castShadow = true;
    scene.add(lightDark);

    const containerEarth = new THREE.Object3D();
    containerEarth.rotateZ(-23.4 * Math.PI / 180);
    containerEarth.position.z = 0;
    scene.add(containerEarth);

    const earthMesh = createMeshEarth();
    earthMesh.receiveShadow = true;
    earthMesh.castShadow = true;
    containerEarth.add(earthMesh);
    onRenderLoop.push((delta) => {
      earthMesh.rotation.y += 1 / 64 * delta;
    });

    const earthCloudMesh = createMeshEarthCloud();
    onRenderLoop.push((delta) => {
      earthCloudMesh.rotation.x += 1 / 48 * delta;
      earthCloudMesh.rotation.y += 1 / 48 * delta;
    });
    earthMesh.add(earthCloudMesh);

    const renderer = new THREE.WebGLRenderer({
      antialias: true,
      alpha: true,
    });
    renderer.shadowMap.enabled = true;

    const onResize = () => {
      const { width, height } = this.$el.getBoundingClientRect();

      camera.aspect = window.innerWidth / window.innerHeight;
      camera.updateProjectionMatrix();
      renderer.setSize(width, height);
    };

    window.addEventListener('resize', onResize, false);
    window.addEventListener('load', () => {
      onResize();
    }, false);

    const { width, height } = this.$el.getBoundingClientRect();

    renderer.setSize(width, height);
    this.$el.appendChild(renderer.domElement);

    let lastTimeMS = null;
    requestAnimationFrame(function animate(nowMS) {
      // keep looping
      requestAnimationFrame(animate);
      // measure time
      lastTimeMS = lastTimeMS || nowMS - 1000 / 60;
      const deltaMS = Math.min(200, nowMS - lastTimeMS);
      lastTimeMS = nowMS;
      // call each update function
      onRenderLoop.forEach((onRenderFct) => {
        onRenderFct(deltaMS / 1000, nowMS / 1000);
        renderer.render(scene, camera);
      });
    });
  },
};
</script>

<style scoped lang="scss">
  .earth {
    position: absolute;
    left: 20vw;
    bottom: 1vh;
    width: 30vw;
    height: 30vh;
  }
</style>
