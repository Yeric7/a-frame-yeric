<script setup>
import { ref, onMounted, getCurrentInstance } from "vue";

import TheCameraRig from "./TheCameraRig.vue";
import TheNavMesh from "./TheNavMesh.vue";
import TheMainRoom from "./TheMainRoom.vue";
import TheLifeCubeRoom from "./TheLifeCubeRoom.vue";

import "../aframe/animation-mixer";
import "../aframe/life-like-automaton.js";
import "../aframe/life-like-automaton2.js";
import "../aframe/teleport-camera-rig.js";
import "../aframe/grabbable.js";

/* src="https://rawgit.com/donmccurdy/aframe-postprocessing-component/master/dist/aframe-postprocessing.min.js" */


defineProps({
  scale: Number,
  overlaySelector: String,
});

const allAssetsLoaded = ref(false);
const eatSoundLoaded = ref(false);

const eatSound = new Audio();
eatSound.src = "assets/mushroomSon.mp3";
eatSound.addEventListener("canplaythrough", () => {
  eatSoundLoaded.value = true;
});

const revealSound = new Audio();
revealSound.src = "assets/Reveal.mp3";


const onMushroomGrabbed = () => {
  // Do something when the mushroom is grabbed
  console.log("Mushroom grabbed!");
};

const loopScale = (el) => {
  AFRAME.ANIME({
    targets: "[ref='all']",
    // scale: () => `${AFRAME.ANIME.random(0.99, 1)} ${AFRAME.ANIME.random(1,1)} ${AFRAME.ANIME.random(1, 1)}`,
    // scale: '1.1 1 1',
    // rotation: '0 1 0',
    rotation: () =>
      `${AFRAME.ANIME.random(0.5, 1)} ${AFRAME.ANIME.random(  0,      1     )} ${AFRAME.ANIME.random(1, 1)}`,
    easing: "easeInOutSine",
    direction: "alternate",
    loop: true,
    duration: 1000,
  });
};

const onMushroomEaten = () => {
  // Do something when the mushroom is eaten
  console.log("Mushroom eaten!");

  if (eatSoundLoaded.value) {
    eatSound.play();

    //Player another sound afeter 2 seconds
    setTimeout(() => {
      revealSound.play();
    }, 2000);
  } else {
    console.log(
      "Sound not loaded yet. It will be played once it finished loading."
    );
  }

  // Get the map entity and move it
  const mapEntity = instance.refs.mapEntity;
  const mapEntity2 = instance.refs.mapEntity2;

  // Move to first position
  mapEntity.setAttribute("animation", {
    property: "position",
    to: "0 0 -1155",
    dur: 5000,
    easing: "linear",
    delay: 2500,
  });

  // Wait for the animation to complete and then move to second position
  setTimeout(() => {
    mapEntity.setAttribute("animation", {
      property: "position",
      to: "0 -10000 -1155",
      dur: 1000,
      easing: "linear",
    });
  }, 5000);

  // Get the 2map entity and move it
  mapEntity2.setAttribute("animation", {
    property: "position",
    to: "0 0 -5",
    dur: 17000,
    easing: "linear",
    delay: 200,
  });

  // Get the camera rig entity and scale it
  // const cameraRigEntity = document.querySelector("#camera-rig");
  // cameraRigEntity.setAttribute("scale", "1. 1 1");
  // loopScale(cameraRigEntity);
  const playerHead = document.querySelector("[ref='all']");
  loopScale(playerHead);

  delay: 1500;
};

let instance;

onMounted(() => {
  instance = getCurrentInstance();
});

console.log("animation terminée");



</script>




<template>
  <a-scene renderer="colorManagement: true;" sound fog="type: exponential; color: #AAA">
    <!-- fog="type: linear; color: #DFE4E6" -->
    <a-assets @loaded="allAssetsLoaded = true">
      <!--
        Title: Yeric VR
        Model source:
        Map: "The Living Forest" (https://skfb.ly/69WTw) by proxy_doug / (http://creativecommons.org/licenses/by/4.0/).
        Object : 
        "Stylized mushrooms" (https://skfb.ly/orrFy) by QumoDone / (http://creativecommons.org/licenses/by/4.0/)
        "Musical Notes" (https://skfb.ly/oxIVp) by Gustavo.Fabi / (http://creativecommons.org/licenses/by/4.0/)
        "Glowing Mushroom" (https://skfb.ly/otXoo) by rabbiturnal / (http://creativecommons.org/licenses/by/4.0/)

      -->

      <!-- test -->
      <img id="sky" src="/assets/sky2.jpg" />

      <a-asset-item id="room" src="assets/vr_gallery.glb"></a-asset-item>

      <a-asset-item id="map1" src="assets/MapForestChampi.glb"></a-asset-item>
      <a-asset-item id="map2" src="assets/MApAvec.glb"></a-asset-item>
      <a-asset-item id="map3" src="assets/ilo.glb"></a-asset-item>
      <a-asset-item id="cloud1" src="assets/cloud1.glb"></a-asset-item>
      <a-asset-item id="cloud2" src="assets/cloud2.glb"></a-asset-item>
      <a-asset-item id="cloud3" src="assets/cloud3.glb"></a-asset-item>

      <a-asset-item
        id="map4"
        src="assets/MapAvecAnimauxEtTexture.glb"
      ></a-asset-item>

      <a-asset-item id="mushroom" src="assets/glowing_mushroom.glb"  ></a-asset-item>
      <!--  <audio id="eat-sound" src="assets/mushroomSon.mp3"></audio> -->
      <a-assets-item v-if="eatSoundLoaded"  id="eat-sound"  src="#eat-sound"      ></a-assets-item>

      <!--  <audio src="https://cdn.aframe.io/basic-guide/audio/backgroundnoise.wav" autoplay
      preload></audio> -->

      <a-asset-item        id="son-mushroom"        response-type="arraybuffer"        src="assets/Mushroom.mp3"        preload="auto"      ></a-asset-item>
      <a-asset-item        id="son-flute"        response-type="arraybuffer"        src="assets/Flute.mp3"        preload="auto"      ></a-asset-item>
      <a-asset-item        id="son-macarena"        response-type="arraybuffer"        src="assets/Macarena.mp3"        preload="auto"      ></a-asset-item>
      <a-asset-item        id="son-gandalf"        response-type="arraybuffer"        src="assets/Gandalf.mp3"        preload="auto"      ></a-asset-item>
      <a-asset-item        id="son-reveal"        response-type="arraybuffer"        src="assets/Reveal.mp3"        preload="auto"      ></a-asset-item>
      <a-asset-item        id="son-portal"        response-type="arraybuffer"        src="assets/Portal.mp3"        preload="auto"      ></a-asset-item>

      <video id="video" src="/assets/videotexture.mp4" autoplay="true"></video>
    </a-assets>

    <a-sky src="#sky" v-if="allAssetsLoaded"></a-sky>

    <!-- <a-entity
    geometry="primitive: box"
      v-if="allAssetsLoaded"
      gltf-model="#map4"
      rotation="0 90 0"
      position="0 0 -50"
      scale="1 1.1 1"
      life-like-automaton
    >    </a-entity> -->


<!--     <a-entity geometry="primitive: plane "
      v-if="allAssetsLoaded"
      
      position="5 5 -4" rotation="-90 0 0" width="1000" height="0"
           life-like-automaton
    > -->
    

<!-- <a-sky src="#sky" v-if="allAssetsLoaded"   position="-11 10 -4.8" scale="0.1 0.1 0.1" life-like-automaton > </a-sky> -->
 

<a-entity   v-if="allAssetsLoaded" ref="all"> 
     

<!--  marche pas <a-entity camera postprocessing="type: vignette; offset: 0.8; darkness: 1.1;"></a-entity> -->

    <a-entity
      v-if="allAssetsLoaded"
      gltf-model="#map1"
      rotation="0 90 0"
      position="0 0 -5"
      scale="1 1.1 1"
      ref="mapEntity"
    >
      <a-entity
        sound="src: #son-mushroom; autoplay: true; maxDistance: 200; loop: true; volume: 10; on: loaded"
        position="8 10 -9"
      ></a-entity>
    </a-entity>

    <a-entity
      v-if="allAssetsLoaded"
      gltf-model="#map2"
      rotation="0 90 0"
      position="0 0 -11155"
      scale="1 1.1 1"
      animation-mixer
      ref="mapEntity2"
    >
      <a-entity
        sound="src: #son-flute; autoplay: true; maxDistance: 3; loop: true; volume: 2; on: loaded"
        position="8 1.5 -9"
      ></a-entity>
      <a-entity
        sound="src: #son-macarena; autoplay: true; maxDistance: 3; loop: true; volume: 2; on: loaded"
        position="-1 1.5 0"
      ></a-entity>
      <a-entity
        sound="src: #son-gandalf; autoplay: true; maxDistance: 10; loop: true; volume: 2; on: loaded"
        position="25 1.5 -8"
      ></a-entity>

      <a-entity
        sound="src: #son-portal; autoplay: true; maxDistance: 10; loop: true; volume: 2; on: loaded"
        position="7 1.5 -19"
      ></a-entity>

      <a-plane position="11 0.2 -4.8" rotation="-90 -90 0" width="40" height="40"  life-like-automaton2></a-plane>
      <a-sphere position="0 1.25 -5" radius="35" life-like-automaton></a-sphere>
    </a-entity>

    <a-entity
      v-if="allAssetsLoaded"
      gltf-model="#map3"
      rotation="0 90 0"
      position="0 0 -5"
      scale="1 1.1 1"
    >
    </a-entity>

    <a-entity
      v-if="allAssetsLoaded"
      gltf-model="#mushroom"
      position="-6.633 0.5 -13.754"
      scale="0.5 0.5 0.5"
      animation="property: rotation; to: 0 360 0; loop: true; dur: 3000"
      id="mushroom-entity"
      grabbable="target: #hand-right;"
      clickable
      v-on:grabbed="onMushroomGrabbed"
      v-on:eaten="onMushroomEaten"
      sound="src: #eat-sound; on: eaten"
    ></a-entity>

    <!--     Clouds:
 -->
    <a-entity
      v-if="allAssetsLoaded"
      gltf-model="#cloud1"
      rotation="0 0 0"
      position="0 0 -35"
      scale="10 10 10"
    >
    </a-entity>

    <a-entity
      geometry="primitive: box"
      material="envMap: #sky; roughness: 10"
    ></a-entity>


  </a-entity>
    <!-- animation="property: rotation; to: 0 360 0; loop: true; dur: 3000"
 -->
    <TheNavMesh />

    <TheCameraRig> </TheCameraRig>
  </a-scene>
</template>

