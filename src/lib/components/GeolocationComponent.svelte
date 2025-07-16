<script lang="ts">
  import { onMount } from 'svelte';
  import { Geolocation } from '@capacitor/geolocation';
  
  let latitude = $state(null);
  let longitude = $state(null);
  let loading = $state(false);
  let error = $state('');
  
  onMount(() => {
    // 可以在這裡初始化
  });
  
  async function getCurrentPosition() {
    try {
      loading = true;
      error = '';
      
      const position = await Geolocation.getCurrentPosition({
        enableHighAccuracy: true
      });
      
      latitude = position.coords.latitude;
      longitude = position.coords.longitude;
    } catch (e) {
      error = '無法獲取位置資訊。請確保已授予位置權限。';
      console.error('位置錯誤', e);
    } finally {
      loading = false;
    }
  }
</script>

<div class="flex flex-col gap-4 p-4">
  <button 
    on:click={getCurrentPosition}
    disabled={loading}
    class="px-4 py-2 bg-blue-500 text-white rounded-lg flex items-center justify-center gap-2 disabled:opacity-50"
  >
    <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
      <circle cx="12" cy="12" r="10"/>
      <polygon points="16.24 7.76 14.12 14.12 7.76 16.24 9.88 9.88 16.24 7.76"/>
    </svg>
    {loading ? '獲取中...' : '獲取當前位置'}
  </button>
  
  {#if error}
    <div class="text-red-500 text-sm">{error}</div>
  {/if}
  
  {#if latitude !== null && longitude !== null}
    <div class="bg-gray-100 p-4 rounded-lg">
      <h3 class="font-semibold text-gray-800 mb-2">您的位置</h3>
      <p class="text-sm">緯度: {latitude.toFixed(6)}</p>
      <p class="text-sm">經度: {longitude.toFixed(6)}</p>
      <p class="text-sm mt-2">
        <a 
          href={`https://maps.google.com/?q=${latitude},${longitude}`} 
          target="_blank" 
          rel="noopener noreferrer"
          class="text-blue-500 underline"
        >
          在 Google 地圖中查看
        </a>
      </p>
    </div>
  {/if}
</div>