<template>
  <div>
    <input type="file" @change="handleFileChange" />

    <div class="d-flex flex-column align-content-center" v-if="previewUrl">
      <div>
        <h4><strong>Taille de l'image:</strong></h4>
        <div>
          <div class="d-flex">
            <h6>Longueur: {{ (height ?? 0) / 10 }} centimètres</h6>

            <button @click="changeSize((height ?? 0) + 100, (width ?? 0))">
              <FontAwesomeIcon :icon="faPlus" />
            </button>
            <button @click="changeSize((height ?? 0) - 100, (width ?? 0))">
              <FontAwesomeIcon :icon="faMinus" />
            </button>
          </div>

          <div class="d-flex">
            <h6>Largeur: {{ (width ?? 0) / 10 }} centimètres</h6>
            <button @click="changeSize((height ?? 0), (width ?? 0) + 100)">
              <FontAwesomeIcon :icon="faPlus" />
            </button>
            <button @click="changeSize((height ?? 0), (width ?? 0) - 100)">
              <FontAwesomeIcon :icon="faMinus" />
            </button>
          </div>
        </div>
      </div>
      <div>
        <h4><strong>Taille de la photo:</strong></h4>
        <div class="d-flex justify-content-between">
          <div class="d-flex flex-column">
            <h6>Largeur: {{ (width ?? 0) / 10 }} centimètres</h6>
            <button @click="changeSizePic((width ?? 0) + 100)">
              <FontAwesomeIcon :icon="faPlus" />
            </button>
            <button @click="changeSizePic((width ?? 0) - 100)">
              <FontAwesomeIcon :icon="faMinus" />
            </button>
          </div>
          <div class="d-flex flex-column">
            <h6>Positionnement Haut-Bas:</h6>
            <button @click="changeSizeMargin(marginTop + 10, marginLeft)">
              <FontAwesomeIcon :icon="faPlus" />
            </button>
            <button @click="changeSizeMargin(marginTop - 10, marginLeft)">
              <FontAwesomeIcon :icon="faMinus" />
            </button>
          </div>
          <div class="d-flex flex-column">
            <h6>Positionnement Gauche-Droite:</h6>
            <button @click="changeSizeMargin(marginTop, marginLeft + 10)">
              <FontAwesomeIcon :icon="faPlus" />
            </button>
            <button @click="changeSizeMargin(marginTop, marginLeft - 10)">
              <FontAwesomeIcon :icon="faMinus" />
            </button>
          </div>
        </div>
      </div>
    </div>
    <div class="frame" :style="frameStyle">
      <div v-if="previewUrl">
        <img :src="previewUrl" alt="Preview" :style="imgStyle" class="img" />
      </div>
    </div>
  </div>
</template>
<script setup  lang="ts">
import { ref, computed } from "vue";
import { FontAwesomeIcon } from "@fortawesome/vue-fontawesome";
import {
  faArrowDown,
  faArrowLeft,
  faArrowRight,
  faArrowUp,
  faMinus,
  faPlus,
} from "@fortawesome/free-solid-svg-icons";
faArrowLeft;
faArrowRight;
faArrowUp;
faArrowDown;
 faPlus;
const file = ref(null);
const previewUrl = ref<string|undefined>(undefined);
const picWidth = ref(500);
const marginTop = ref(0);
const marginLeft = ref(0);
const height = ref<number|undefined>(undefined);
const width = ref<number|undefined>(undefined);

const handleFileChange = (event:any) => {
  const selectedFile = event.target.files[0];
  if (selectedFile) {
    file.value = selectedFile;
    previewUrl.value = URL.createObjectURL(selectedFile);
    const reader = new FileReader();

      reader.onload = (e) => {
        const img = new Image();
        img.onload = () => {
          width.value = img.width;
          height.value = img.height;
        };
        img.src = e.target?.result as string;
      };

      //reader.readAsDataURL(file);
  }
};
const frameStyle = computed(() => {
  return {
    "--previewUrl": "url(" + previewUrl.value + ")",
  };
});

const imgStyle = computed(() => {
  return {
    width: width.value ? width.value + "px":'auto',
    height: height.value? height.value + "px":'auto',
  };
});
const changeSize = (newHeight:number, newWidth:number) => {
  width.value = newWidth;
  height.value = newHeight;
};
const changeSizePic = (newWidth:number) => {
  console.log(newWidth);

  picWidth.value = newWidth;
};
const changeSizeMargin = (newMarginTop:number, newMarginLeft:number) => {
  marginTop.value = newMarginTop;
  marginLeft.value = newMarginLeft;
};
</script>

<style lang="scss">
.frame {
  --previewUrl: unset;

  position: relative;
  margin: auto;
  margin-top: 100px;
  border: solid 1px grey;
  width: 100%;
  height: 80vh;
overflow: visible;
background-image: url("https://plus.unsplash.com/premium_photo-1661766077694-6e3750b0fb97?fm=jpg&q=60&w=3000&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxzZWFyY2h8MXx8ZW1wdHklMjB3YWxsfGVufDB8fDB8fHww");
 background-size: cover;
 background-repeat: no-repeat;
 
}

.img {
  position: absolute;
  margin: auto;
 /// mask-image: url("/public/frames/tryptych_40x60.svg");
  mask-repeat: no-repeat;
  mask-position: center;
  // mask-mode: alpha; /* Use alpha channel for masking */
  // mask-origin: content-box; /* Mask only applied inside content-box */
  // mask-clip: padding-box; /* Mask affects padding-box area */
mask-size: 130px 60px;
border: solid 1px yellow;

}
</style>
