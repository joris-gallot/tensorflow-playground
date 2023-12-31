<script lang="ts" setup>
import "@tensorflow/tfjs";
import * as mobilenet from "@tensorflow-models/mobilenet";

const imagePredictions = ref();

async function predictImage(image: HTMLImageElement) {
  const model = await mobilenet.load();

  const predictions = await model.classify(image);

  imagePredictions.value = predictions;
}

function setImage(event: Event) {
  const [file] = (event.target as HTMLInputElement)?.files || [];

  if (!file) {
    return;
  }

  const fr = new FileReader();

  fr.onload = function () {
    const image = document.querySelector<HTMLImageElement>("#imageElement");

    if (!image) {
      return;
    }

    image.src = fr.result as string;

    predictImage(image);
  };

  fr.readAsDataURL(file);
}
</script>

<template>
  <div>
    <pre>predictions: {{ imagePredictions }}</pre>
    <input type="file" @change="setImage" />
    <img id="imageElement" />
  </div>
</template>
