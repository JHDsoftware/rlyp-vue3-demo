<script setup>
// This starter template is using Vue 3 <script setup> SFCs
// Check out https://v3.vuejs.org/api/sfc-script-setup.html#sfc-script-setup
import set from './assets/set.json'
import { computed, reactive, ref } from 'vue'

const currentKey = ref(Object.keys(set)[0])
const selectedImage = reactive({})
console.log(selectedImage)

function updateCurrentKey (key) {
  console.log(set[currentKey.value])
  currentKey.value = key
}

function selectPicture (pic) {
  selectedImage[currentKey.value] = pic
}

function random () {
  for (const key in set) {
    const arr = set[key]
    selectedImage[key] = arr[getRndInteger(0, arr.length - 1)]
  }
}

function getRndInteger (min, max) {
  return Math.floor(Math.random() * (max - min + 1)) + min
}

const displayImages = computed(() => {
  return set[currentKey.value]
})

</script>

<template>
  <div style="display: grid;grid-template-columns: repeat(2,1fr)">
    <div class="result" style="position:relative;">
      <n-image width="300" style="position: absolute;top:0;left: 0;" v-for="key in Object.keys(selectedImage).sort()"
               :src="'./layers/'+key+'/'+selectedImage[key]"
      ></n-image>

    </div>
    <div class="panel" style="display: flex">
      <div>
        <n-button-group vertical>
          <n-button @click="random" type="primary"> Random</n-button>
          <template v-for="keys in Object.keys(set)">
            <n-button @click="updateCurrentKey(keys)">{{ keys }}</n-button>
          </template>
        </n-button-group>
      </div>
      <div>
        {{ currentKey }}
        <n-image-group>
          <n-image @click="selectPicture(image)"
                   preview-disabled
                   v-for="image in displayImages" :key="image" width="100"
                   :src="'./layers/'+currentKey+'/'+image"></n-image>
        </n-image-group>
      </div>
    </div>
  </div>
</template>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
