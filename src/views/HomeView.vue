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

              <!-- 1cm == 3px -->
              <h6>Largeur: {{ dimensions.width }} px</h6>
              <h6>Largeur: {{ (dimensions.width / 3).toFixed(2)}} cm</h6>
            </div>
            <div class="d-flex">
              <button @click="changeSize(dimensions.width + 50)">+</button>
              <button @click="changeSize(dimensions.width - 50)">-</button>
            </div>
          </div>
          <!-- <div>
            <h4><strong>Position de l'image:</strong></h4>
        
            <div class="d-flex">
              <button @click="changePlan(true)">Arriere plan</button>
              <button @click="changePlan(false)">Premier plan</button>
            </div>
          </div> -->
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
        <div class="frame" id="frame" @mousedown="startDrag" @touchstart="startDrag"></div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
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

const imageRef = ref < HTMLImageElement|null > (null);
const isDragging = ref(false);
const offsetX = ref(0);
const offsetY = ref(0);
const zindex = ref(0);
const position = ref({ x: 50, y: 50 });

const file = ref(null);
const previewUrl = ref<string|undefined>(undefined);

const dimensions = ref({ width: 0, height: 0 });
const imageStyle = computed<any>(() => ({
  zIndex:zindex.value,
  left: `${position.value.x}px`,
  top: `${position.value.y}px`,
  position: "absolute", // Explicitly typed as Partial<CSSStyleDeclaration>
  cursor: isDragging.value ? "grabbing" : "grab",
  width: dimensions.value.width + "px",
  backgroundImage: "url(" + previewUrl.value + ")",
}));
const handleFileChange = (event:any) => {
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

const startDrag = (event :any) => {
  isDragging.value = true;
  const dragArea = document.getElementById("frame") as Node;
      // Allow dragging if clicking the image or if the image is inside #drag-area
      console.log(imageStyle);
      console.log(dragArea);
      
      
      if (
         !imageRef.value &&
         (!dragArea.contains(event.target) )
      ) {
        return; // Exit if the click is outside the image and #drag-area
      }


  const clientX = "touches" in event ? event.touches[0].clientX : event.clientX;
  const clientY = "touches" in event ? event.touches[0].clientY : event.clientY;

  if (imageRef.value || dragArea.contains(event.touches)) {
    offsetX.value = clientX - imageRef.value!.offsetLeft;
    offsetY.value = clientY - imageRef.value!.offsetTop;
  }

  document.addEventListener("mousemove", moveImage);
  document.addEventListener("mouseup", stopDrag);
  document.addEventListener("touchmove", moveImage);
  document.addEventListener("touchend", stopDrag);
};

const moveImage = (event:any) => {
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

const changeSize = (newWidth:number) => {
  dimensions.value.width = newWidth;
};

</script>

<style scoped lang="scss">
@mixin respond($breakpoint) {
  @if $breakpoint==phone {
    @media (max-width: 500px) {
      /* 600px */
      @content;
    }
  }
  @if $breakpoint==tablet {
    @media (min-width: 501px) {
      /* 900px */
      @content;
    }
  }
  @if $breakpoint==tablet-land {
    @media (min-width: 800px) {
      /* 1100px */
      @content;
    }
  }
  @if $breakpoint==desk {
    @media (min-width: 1024px) {
      /* 1800px */
      @content;
    }
  }
  @if $breakpoint==big-desk {
    @media (min-width: 1200px) {
      /* 1800px */
      @content;
    }
  }
}


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
  width: 50%;
  height: 500px;
  overflow: visible;
  /* background-image: url("https://plus.unsplash.com/premium_photo-1661766077694-6e3750b0fb97?fm=jpg&q=60&w=3000&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxzZWFyY2h8MXx8ZW1wdHklMjB3YWxsfGVufDB8fDB8fHww"); */
  background-image: url("/public/frames/B_tryptich_40x60.svg");

  background-size: cover;
  background-repeat: no-repeat;

  @include respond(phone) {
    width: 100%;
    height: 300px;
  }
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
