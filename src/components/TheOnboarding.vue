<script setup>
  import { ref } from 'vue';

  defineProps({
    loaded: Boolean,
  });

  const showOnboarding = ref(true);

  function enterScene() {
    showOnboarding.value = false;
    if (AFRAME.utils.device.checkHeadsetConnected() && !AFRAME.utils.device.isMobile()) {
      document.querySelector('a-scene').enterVR();
    }
  }
</script>

<template>
  <div id="onboarding" v-if="showOnboarding">
    <div>
      <h1>Yeric VR - The mushroom project</h1>
      <p v-if="!loaded">loading...</p>
      <button v-if="loaded" @click="enterScene()">Enter scene</button>
      <div class="licences">
        <dl>
          <dt>
            <i>Included</i>

              
          </dt>
          <dt>
            <a href="https://github.com/c-frame/aframe-extras" target="_blank">aframe-extras controls and animation-mixer</a>
          </dt><dd>
            <a href="https://github.com/c-frame/aframe-extras/blob/master/LICENSE" target="_blank">MIT License</a>
          </dd>

          <dt>
            <a href="https://github.com/jure/aframe-blink-controls/" target="_blank">aframe-blink-controls</a>
          </dt><dd>
            <a href="https://github.com/jure/aframe-blink-controls/blob/main/LICENSE" target="_blank">MIT License</a>
          </dd>

          <dt>
            <a href=" https://github.com/AdaRoseCannon/aframe-xr-boilerplate" target="_blank">simple-navmesh-constraint</a>
          </dt><dd>
            By Ada Rose Cannon under MIT License
          </dd>

          <dt>
            <a href="https://skfb.ly/69WTw" target="_blank">The Living Forest by proxy_doug</a>
         </dt><dd>
            under <a href="(http://creativecommons.org/licenses/by/4.0/)" target="_blank">CC BY 4.0</a>
          </dd>
          <dt>
            <a href="https://skfb.ly/orrFy" target="_blank">Stylized mushrooms by QumoDone</a>
         </dt><dd>
            under <a href="(http://creativecommons.org/licenses/by/4.0/)" target="_blank">CC BY 4.0</a>
          </dd>
          <dt>
            <a href="https://skfb.ly/otXoo" target="_blank">Glowing Mushroom by rabbiturnal</a>
         </dt><dd>
            under <a href="(http://creativecommons.org/licenses/by/4.0/)" target="_blank">CC BY 4.0</a>
          </dd>
          <dt>
            <a href="https://skfb.ly/oxIVp" target="_blank">Musical Notes by Gustavo.Fabi</a>
         </dt><dd>
            under <a href="(http://creativecommons.org/licenses/by/4.0/)" target="_blank">CC BY 4.0</a>
          </dd>
          <dt>
            <a >   </a>
         </dt><dd>
            under <a>MAP</a>
          </dd>
        </dl>
    
      </div>
    </div>
  </div>
</template>

<!-- https://www.youtube.com/watch?v=IwLSrNu1ppI 
Vidéo de Dan Cristian Pădureț: https://www.pexels.com/fr-fr/video/texture-abstrait-colore-mouvement-4365142/-->

<style scoped>
  h1 { font-size: 1.5rem }
  a {
    color: #eee;
    text-decoration: none;
  }
  .licences {
    margin: 2rem 0;
    font-size: 1rem;
    text-align: left;
  }
  .licences dt {
    padding-top: 0.75rem;
    font-size: 0.9rem;
    font-weight: bold;
  }
  .licences dd {
    margin-left: 0;
    font-size: 0.8rem;
  }

  #onboarding {
    position: absolute;
    top: 0;
    left: 0;
    background-color: #333;
    color: #ccc;
    width: 100vw;
    height: 100vh;
    padding: 1rem;
    font-family: monospace;
    z-index: 10000;
    overflow: auto;
  }
  #onboarding > * {
    margin: 0 auto;
    max-width: 50rem;
    width: calc(100vw - 10rem);
    text-align: center;
    border-radius: 0.3rem;
    padding: 1rem;
    font-size: 1.3rem;
  }
  #onboarding button {
    font-size: 1.3rem;
    padding: 0.5rem 1rem;
    border-radius: 0.3rem;
    background-color: white;
    color: black;
    border: none;
    cursor: pointer;
  }
</style>