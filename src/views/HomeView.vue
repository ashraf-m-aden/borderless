<template>
  <div>
    <input type="file" @change="handleFileChange" />

    <div class="d-flex flex-column align-content-center">
      <div>
        <h4><strong>Taille du cadre:</strong></h4>
        <div>
          <div class="d-flex">
            <h6>Longueur: {{ height / 10 }} centimètres</h6>

            <button @click="changeSize(height + 100, width)">
              <FontAwesomeIcon :icon="faPlus" />
            </button>
            <button @click="changeSize(height - 100, width)">
              <FontAwesomeIcon :icon="faMinus" />
            </button>
          </div>

          <div class="d-flex">
            <h6>Largeur: {{ width / 10 }} centimètres</h6>
            <button @click="changeSize(height, width + 100)">
              <FontAwesomeIcon :icon="faPlus" />
            </button>
            <button @click="changeSize(height, width - 100)">
              <FontAwesomeIcon :icon="faMinus" />
            </button>
          </div>
        </div>
      </div>
      <div>
        <h4><strong>Taille de la photo:</strong></h4>
        <div class="d-flex justify-content-between">
          <div class="d-flex flex-column">
            <h6>Largeur: {{ width / 10 }} centimètres</h6>
            <button @click="changeSizePic(width + 100)">
              <FontAwesomeIcon :icon="faPlus" />
            </button>
            <button @click="changeSizePic(width - 100)">
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
        <img :src="previewUrl" alt="Preview" :style="imgStyle" />
      </div>
    </div>
  </div>
</template>
<script setup>
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
faMinus, faPlus;
const file = ref(null);
const previewUrl = ref(null);
const width = ref(500);
const picWidth = ref(500);
const marginTop = ref(0);
const marginLeft = ref(0);
const height = ref(500);
const handleFileChange = (event) => {
  const selectedFile = event.target.files[0];
  if (selectedFile) {
    file.value = selectedFile;
    previewUrl.value = URL.createObjectURL(selectedFile);
  }
};
const frameStyle = computed(() => {
  return {
    "--previewUrl": "url(" + previewUrl.value + ")",
    width: width.value + "px",
    height: height.value + "px",
  };
});

const imgStyle = computed(() => {
  return {
    width: picWidth.value + "px",
    "margin-top": marginTop.value + "px",
    "margin-left": marginLeft.value + "px",
  };
});
const changeSize = (newHeight, newWidth) => {
  width.value = newWidth;
  height.value = newHeight;
};
const changeSizePic = (newWidth) => {
  console.log(newWidth);
  
  picWidth.value = newWidth;
};
const changeSizeMargin = (newMarginTop, newMarginLeft) => {
  marginTop.value = newMarginTop;
  marginLeft.value = newMarginLeft;
};
</script>

<style lang="scss">
.frame {
  --previewUrl: unset;

 
  margin: auto;
  margin-top: 100px;
  border: solid 1px grey;

  mask-image: url("/public/frames/tryptych_40x60.svg");
}
</style>
