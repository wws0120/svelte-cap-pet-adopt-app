<script lang="ts">
  import { onMount } from 'svelte';
  import { Camera, CameraResultType, CameraSource } from '@capacitor/camera';
  
  let photoUrl = $state('');
  let isNative = $state(false);
  
  onMount(() => {
    // 檢查是否在原生環境中運行
    isNative = window.Capacitor && window.Capacitor.isNative;
  });
  
  async function takePicture() {
    try {
      const image = await Camera.getPhoto({
        quality: 90,
        allowEditing: true,
        resultType: CameraResultType.Uri,
        source: CameraSource.Camera
      });
      
      photoUrl = image.webPath;
    } catch (error) {
      console.error('無法拍攝照片', error);
    }
  }
</script>

<div class="flex flex-col items-center gap-4 p-4">
  {#if photoUrl}
    <img src={photoUrl} alt="拍攝的照片" class="w-full max-w-md rounded-lg shadow-md" />
  {/if}
  
  <button 
    on:click={takePicture}
    class="px-4 py-2 bg-blue-500 text-white rounded-lg flex items-center justify-center gap-2"
  >
    <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
      <path d="M14.5 4h-5L7 7H4a2 2 0 0 0-2 2v9a2 2 0 0 0 2 2h16a2 2 0 0 0 2-2V9a2 2 0 0 0-2-2h-3l-2.5-3z"/>
      <circle cx="12" cy="13" r="3"/>
    </svg>
    拍攝照片
  </button>
  
  {#if !isNative}
    <p class="text-sm text-gray-500">
      注意：完整的相機功能在原生應用程式中可用。在瀏覽器中，將使用網頁相機。
    </p>
  {/if}
</div>