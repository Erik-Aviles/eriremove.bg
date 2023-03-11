<script lang="ts">
  import "two-up-element";
  import { originalImage, modifiedImage } from "./store";
  let processImgImage = true;
  let tries = 0;
  let intervalId: any;

  $: {
    if (processImgImage) {
      clearInterval(intervalId);
      intervalId = setInterval(() => {
        tries++;
        const img = new Image();
        img.src = $modifiedImage;
        img.onload = () => {
          processImgImage = false;
          clearInterval(intervalId);
        };
      }, 500);
    }
  }
</script>

<two-up>
  <img src={$originalImage} alt="Imagen oringinal subida por el usuario" />
  {#if processImgImage}
    <div class="flex flex-col justify-center items-center">
      <p class="text-center mt-4">Procesando imagen...</p>
    </div>
  {:else}
    <img
      src={`${$modifiedImage}&t=${tries}`}
      alt="Imagen sin fonfo subida por el usuario"
    />
  {/if}
</two-up>

<a
  download
  href={$modifiedImage}
  class="block bg-blue-500 hover:bg-blue-700 text-xl text-center w-full font-bold  text-white rounded-full px-4 py-2 mt-10"
>
  Descargar imagen sin fondo
</a>
