<script lang="ts">
  import StepUpload from "./StepUpload.svelte";
  import ClaudinaryLogo from "./ClaudinaryLogo.svelte";
  import { imageStatus } from "./store";
  import { ImageStatus } from "../type.d";
  import StepEdit from "./StepEdit.svelte";

</script>

<div
  class="max-w-xl m-auto grid grid-cols-1 place-content-center w-full h-screen p-4"
>
  <header class="flex justify-center py-10">
    <h1 class="text-3xl font-bold text-blue-900 tracking-tighter">
      Mi app eriremove<span class="text-blue-600">-bg</span>
    </h1>
  </header>

  <main class="w-full block">
    {#if $imageStatus === ImageStatus.READY || $imageStatus === ImageStatus.UPLOADING}
    <StepUpload />    
    {:else if $imageStatus === ImageStatus.DONE}
     <StepEdit />
    {/if}
  </main>

  <footer class="flex justify-center aling-items-center gap-x-2 font-semibold pt-16">
    <p>Hecho con</p>
    <a href="https://cloudinary" target="_blank" rel="noreferrer"
      ><ClaudinaryLogo />
    </a>
  </footer>
</div>
