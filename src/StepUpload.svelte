<script lang="ts">
  import { Cloudinary } from "@cloudinary/url-gen";
import { backgroundRemoval} from "@cloudinary/url-gen/actions/effect";

  import { ImageStatus } from "../type.d";
  import { imageStatus, modifiedImage, originalImage } from "./store";
  import Dropzone from "dropzone";
  import "dropzone/dist/dropzone.css";
  import { onMount } from "svelte";

  const cloudinary = new Cloudinary({
    cloud: {
      cloudName: "dutf6po0b",
    },
    url: {
      secure: true,
    }
  }) 

  const URL = "https://api.cloudinary.com/v1_1/dutf6po0b/image/upload";

  onMount(() => {
    const dropzone = new Dropzone("#dropzone", {
      uploadMultiple: false,
      acceptedFiles: ".jpg,.png,.webp",
      maxFiles: 1,
    });

    /**
     * Envia las imagenes
     */
    dropzone.on("sending", (file, xhr, formData) => {
      imageStatus.set(ImageStatus.UPLOADING);
      //Aqui se añade la apiKey, configuracion
      formData.append("upload_preset", "mi_eriremove");
      formData.append("timestamp", Date.now() / 1000);
      formData.append("api_key", 292493933198416);
    });

    dropzone.on("success", (file, response) => {
      const { public_id: publicId, secure_url: url } = response;

      const imageWithOutBackgroud = cloudinary.image(publicId).effect(backgroundRemoval())

        
        imageStatus.set(ImageStatus.DONE);
        modifiedImage.set(imageWithOutBackgroud.toURL());
        originalImage.set(url);
        
        
    });

    dropzone.on("error", (file, response) => {
      console.log("Hubo error en carga");
      console.log(response);
    });
  });
</script>

<!-- Example of a form that Dropzone can take over -->
<form
  id="dropzone"
  action="https://api.cloudinary.com/v1_1/dutf6po0b/image/upload"
  class="shadow-2xl border-dashed border-2 border-gray-300 rounded-lg aspect-video w-full flex items-center justify-center flex-col"
>
  {#if $imageStatus === ImageStatus.READY}
    <button
      class="pointer-events-none bg-blue-600 rounded-full text-bold text-white text-xl px-6 py-4"
      >Cargar imagen
    </button>
    <strong class="text-lg mt-4 text-gray-700">o soltar un archivo</strong>
  {/if}

  {#if $imageStatus === ImageStatus.UPLOADING}
  <strong class="text-lg mt-4 text-gray-700">Subiendo archivo...</strong>
  {/if}


</form>

<!-- https://api.cloudinary.com/demo/image/upload -->
