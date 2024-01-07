<script setup>
import { onMounted, ref } from 'vue';
import { useRoute } from 'vue-router';
import { fabric } from 'fabric';
import Modal from './Modal.vue';

const route = useRoute();
const canvas = ref(null);
const moded = ref(null);
const brushMode = ref(false);
const brushObj = ref({});
const textMode = ref(false);
const textObj = ref({});

onMounted(() => {
  brushObj.value.active = false;

  let data = new URL(`../assets/images/${route.params.id}`, import.meta.url)
    .href;
  canvas.value = new fabric.Canvas('canvas', {
    backgroundImage: data,
    isDrawingMode: false,
  });

  //   fabric.Image.fromURL(data, function (img) {
  //     fab.value.add(img);
  //   });
});

function showBrush() {
  if (canvas.value.isDrawingMode == true) {
    canvas.value.isDrawingMode = false;
    brushObj.value.active = false;
  } else {
    brushMode.value = true;
    brushObj.value.active = true;
  }
}

function brush() {
  brushMode.value = false;
  canvas.value.isDrawingMode = true;
  canvas.value.freeDrawingBrush.color = brushObj.value.color ?? 'black';
  canvas.value.freeDrawingBrush.width = brushObj.value.width ?? 10;
  canvas.value.renderAll();
}

function showText() {
  if (moded.value == 'text') {
    textObj.value.active = false;
  } else {
    textMode.value = true;
    textObj.value.active = true;
  }
}

function deleteObject() {
  if (
    canvas.value.getActiveObject() == undefined ||
    canvas.value.getActiveObject() === null
  ) {
    alert('Please selected object for deleted');
  } else {
    canvas.value.remove(canvas.value.getActiveObject());
  }
}

function addText() {
  let text = textObj.value.text;

  if (text == null || text == '') {
    alert('Input text kosong');
  }

  let fabricText = new fabric.Text(text, {
    fill: textObj.value.color ?? 'black',
    fontFamily: textObj.value.font ?? 'Calibri',
    fontSize: textObj.value.sizing ?? 60,
    stroke: textObj.value.stroke ?? 'black',
    strokeWidth: textObj.value.strokeWidth ?? 1,
    fontWeight: textObj.value.bold ?? 'normal',
  });

  canvas.value.add(fabricText);
  canvas.value.renderAll();

  textMode.value = false;
  textObj.value.active = false;
}

function download() {
  let text = prompt('Berikan Nama file');

  if (text == null || text == '') {
    alert('Input text kosong');
  }
  let link = document.createElement('a');
  link.download = text + '.jpg';
  link.href = canvas.value.toDataURL('image/jpg');
  link.target = '_blank';
  link.click();
}
</script>

<template>
  <div class="flex flex-row">
    <div class="bg-neutral p-4 w-60 space-y-1 flex flex-col">
      <div class="text-white text-xl">Tools</div>
      <div
        class="text-white btn btn-xs"
        :class="{
          'btn-error': brushObj.active == true,
          'btn-info': brushObj.active == false,
        }"
        @click="showBrush"
      >
        Brush
      </div>
      <div
        class="text-white btn btn-xs btn-info"
        :class="{
          'btn-error': textObj.active == true,
          'btn-info': textObj.active == false,
        }"
        @click="showText"
      >
        Text
      </div>
      <div class="text-white btn btn-xs btn-info" @click="deleteObject">
        Delete
      </div>
      <div class="text-white btn btn-xs btn-info" @click="download">
        Download
      </div>
    </div>
    <div align="center" class="mx-auto pb-4">
      <canvas height="600" width="900" id="canvas">Tidak Ada Gambar</canvas>
    </div>
  </div>

  <Teleport to="body">
    <Modal :show="brushMode" @close="brushMode = false">
      <template #header>
        <h3>BRUSH</h3>
      </template>
      <template #body>
        <div class="flex flex-col">
          <label>Color</label>
          <input type="color" v-model="brushObj.color" />
        </div>
        <div class="flex flex-col">
          <label>Width</label>
          <input type="number" v-model="brushObj.width" placeholder="10" />
        </div>
      </template>
      <template #footer
        ><div class="space-x-2">
          <button
            @click="
              () => {
                brushMode = false;
                brushObj.active = false;
              }
            "
            class="btn btn-error btn-sm text-white"
          >
            Cancel</button
          ><button @click="brush()" class="btn btn-success btn-sm text-white">
            Submit
          </button>
        </div>
      </template>
    </Modal>
  </Teleport>

  <Teleport to="body">
    <Modal :show="textMode" @close="textMode = false">
      <template #header>
        <h3>Add Text</h3>
      </template>
      <template #body>
        <div class="space-y-2">
          <div class="flex flex-col">
            <label>Text</label>
            <input type="text" v-model="textObj.text" placeholder="hello" />
          </div>
          <div class="flex flex-col">
            <label>Color</label>
            <input type="color" v-model="textObj.color" />
          </div>
          <div class="flex flex-col">
            <label>Sizing</label>
            <input type="number" v-model="textObj.sizing" placeholder="60" />
          </div>
          <div class="flex flex-col">
            <label>Font Family</label>
            <select v-model="textObj.font" class="">
              <option disabled selected>Pick your font family</option>
              <option value="Arial">Arial</option>
              <option value="Times New Roman">Times New Roman</option>
              <option value="Calibri">Calibri</option>
              <option value="Pacifico">Pacifico</option>
              <option value="VT323">VT323</option>
              <option value="Quicksand">Quicksand</option>
              <option value="Inconsolata">Inconsolata</option>
            </select>
          </div>
          <div class="flex flex-col">
            <label>Border Color</label>
            <input type="color" v-model="textObj.stroke" />
          </div>
          <div class="flex flex-col">
            <label>Border Length</label>
            <input
              type="number"
              v-model="textObj.strokeWidth"
              placeholder="2"
            />
          </div>
          <div class="flex flex-col">
            <label>Bold</label>
            <select v-model="textObj.bold" class="">
              <option disabled selected>normal</option>
              <option value="normal">normal</option>
              <option value="bold">bold</option>
            </select>
          </div>
        </div>
      </template>
      <template #footer
        ><div class="space-x-2">
          <button
            @click="
              () => {
                textMode = false;
                textObj.active = false;
              }
            "
            class="btn btn-error btn-sm text-white"
          >
            Cancel</button
          ><button @click="addText()" class="btn btn-success btn-sm text-white">
            Submit
          </button>
        </div>
      </template>
    </Modal>
  </Teleport>
</template>
