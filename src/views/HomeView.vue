<template>
  <div class="container-fluid">
    <div class="row">
      <div class="col-12">
        <h2>Vue 3 Draggable Image</h2>
        <input type="file" @change="handleFileChange" />
      </div>
    </div>
    <div class="row">
      <div class="col-12">
        <div class="text-start" v-if="previewUrl">
          <div class="d-flex">
            <h4><strong>Taille de l'image:</strong></h4>
            <div class="d-flex flex-column">
              <h6>Largeur: {{ dimensions.width }} px</h6>
              <h6>Largeur: {{ dimensions.width / 5 }} cm</h6>
            </div>
            <div class="d-flex">
              <button @click="changeSize(dimensions.width + 100)">+</button>
              <button @click="changeSize(dimensions.width - 100)">-</button>
            </div>
          </div>
          <div>
            <h4><strong>Position de l'image:</strong></h4>
        
            <div class="d-flex">
              <button @click="changePlan(true)">Arriere plan</button>
              <button @click="changePlan(false)">Premier plan</button>
            </div>
          </div>
          <div></div>
        </div>
      </div>
      <div class="col-12">
        <img
          alt="Draggable"
          class="draggable"
          ref="imageRef"
          :src="previewUrl"
          :style="imageStyle"
          @mousedown="startDrag"
          @touchstart="startDrag"
        />
        <div class="frame"></div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from "vue";
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

const imageRef = (ref < HTMLImageElement) | (null > null);
const isDragging = ref(false);
const offsetX = ref(0);
const offsetY = ref(0);
const zindex = ref(0);
const position = ref({ x: 50, y: 50 });

const file = ref(null);
const previewUrl = ref(undefined);

const dimensions = ref({ width: 0, height: 0 });
const imageStyle = computed(() => ({
  zIndex:zindex.value,
  left: `${position.value.x}px`,
  top: `${position.value.y}px`,
  position: "absolute", // Explicitly typed as Partial<CSSStyleDeclaration>
  cursor: isDragging.value ? "grabbing" : "grab",
  width: dimensions.value.width + "px",
  backgroundImage: "url(" + previewUrl.value + ")",
}));
const handleFileChange = (event) => {
  const selectedFile = event.target.files[0];

  if (selectedFile) {
    const img = new Image();
    file.value = selectedFile;
    previewUrl.value = URL.createObjectURL(selectedFile);
    img.src = URL.createObjectURL(selectedFile);
    img.onload = () => {
      img.width = 500;
      dimensions.value = { width: img.width, height: img.height };
      // Don't forget to release the object URL after use
    };
  }
};

const startDrag = (event) => {
  isDragging.value = true;

  const clientX = "touches" in event ? event.touches[0].clientX : event.clientX;
  const clientY = "touches" in event ? event.touches[0].clientY : event.clientY;

  if (imageRef.value) {
    offsetX.value = clientX - imageRef.value.offsetLeft;
    offsetY.value = clientY - imageRef.value.offsetTop;
  }

  document.addEventListener("mousemove", moveImage);
  document.addEventListener("mouseup", stopDrag);
  document.addEventListener("touchmove", moveImage);
  document.addEventListener("touchend", stopDrag);
};

const moveImage = (event) => {
  if (!isDragging.value) return;

  const clientX = "touches" in event ? event.touches[0].clientX : event.clientX;
  const clientY = "touches" in event ? event.touches[0].clientY : event.clientY;

  position.value = {
    x: clientX - offsetX.value,
    y: clientY - offsetY.value,
  };
};

const stopDrag = () => {
  isDragging.value = false;
  document.removeEventListener("mousemove", moveImage);
  document.removeEventListener("mouseup", stopDrag);
  document.removeEventListener("touchmove", moveImage);
  document.removeEventListener("touchend", stopDrag);
};

const changeSize = (newWidth) => {
  dimensions.value.width = newWidth;
};
const changePlan = (isArrierePlan) => {
  console.log(isArrierePlan);
  
  if (isArrierePlan) {
    zindex.value = 0
  } else{
    zindex.value = 99
  }
};
</script>

<style scoped>
.container {
  text-align: center;
  font-family: Arial, sans-serif;
}

.draggable {
  width: 150px;
  height: auto;
  position: absolute;
  cursor: grab;

  /* mask-image: url("/public/frames/tryptych_40x60.svg");
  z-index: 99;
  background-size: cover;
  overflow: hidden; /* Ensures no overflow */
  /* -webkit-mask-image: url("/public/frames/tryptych_40x60.svg");; /* For Safari */
  /* mask-position: center;
  mask-repeat: no-repeat;
  mask-size: 650px 300px; /* Fixed mask size */
  /* outline: 2px black solid;
  clip-path: inset(); */
}
.frame {
  position: relative;
  margin: auto;
  margin-top: 100px;
  width: 100%;
  height: 80vh;
  overflow: visible;
  /* background-image: url("https://plus.unsplash.com/premium_photo-1661766077694-6e3750b0fb97?fm=jpg&q=60&w=3000&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxzZWFyY2h8MXx8ZW1wdHklMjB3YWxsfGVufDB8fDB8fHww"); */
  background-image: url("/public/frames/B_tryptich_40x60.svg");

  background-size: cover;
  background-repeat: no-repeat;
}
.mask {
  position: absolute;
  mask-image: url("/public/frames/Fichier\ 3.svg");
  mask-repeat: no-repeat;
  mask-position: center;
  mask-mode: alpha; /* Use alpha channel for masking */
  mask-origin: content-box; /* Mask only applied inside content-box */
  mask-clip: padding-box; /* Mask affects padding-box area */
  border: solid 1px yellow;
  width: 550px;
  height: 150px;
  top: 100px;
  right: 0;
  left: 0;
  margin: auto;
  background-image: url("https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcR9SRRmhH4X5N2e4QalcoxVbzYsD44C-sQv-w&s");
}
</style>
