<script>
  import { ArrowLeft, ChevronRight, Bell, Shield, HelpCircle, MapPin, User, Heart, LogOut, Camera, Navigation } from 'lucide-svelte';
  import CameraComponent from '$lib/components/CameraComponent.svelte';
  import GeolocationComponent from '$lib/components/GeolocationComponent.svelte';
  import NotificationsComponent from '$lib/components/NotificationsComponent.svelte';
  
  let { navigateTo } = $props();
  let activeTab = $state('settings');

  const settingsItems = [
    { icon: Bell, label: '通知設定', hasToggle: true, tab: 'notifications' },
    { icon: MapPin, label: '位置設定', hasToggle: false, tab: 'location' },
    { icon: Camera, label: '相機設定', hasToggle: false, tab: 'camera' },
    { icon: Shield, label: '隱私與安全', hasToggle: false },
    { icon: Heart, label: '最愛寵物', hasToggle: false },
    { icon: HelpCircle, label: '幫助與支援', hasToggle: false },
    { icon: User, label: '帳戶設定', hasToggle: false },
  ];
  
  function setActiveTab(tab) {
    activeTab = tab;
  }
</script>

<div class="min-h-screen bg-gray-50">
  <!-- Header -->
  <div class="flex justify-between items-center px-4 py-4 bg-white border-b border-gray-100">
    <button on:click={() => navigateTo('listings')}>
      <ArrowLeft class="text-gray-600" size={24} />
    </button>
    <h1 class="text-lg font-semibold text-gray-800">
      {activeTab === 'settings' ? '設定' : 
       activeTab === 'notifications' ? '通知設定' :
       activeTab === 'location' ? '位置設定' :
       activeTab === 'camera' ? '相機設定' : '設定'}
    </h1>
    <div class="w-6"></div>
  </div>

  {#if activeTab === 'settings'}
    <!-- Settings List -->
    <div class="px-4 py-4">
      <div class="space-y-1">
        {#each settingsItems as item}
          <button 
            class="w-full flex items-center justify-between py-4 px-4 hover:bg-gray-50 rounded-lg transition-colors"
            on:click={() => item.tab ? setActiveTab(item.tab) : null}
          >
            <div class="flex items-center gap-3">
              <div class="w-8 h-8 bg-blue-100 rounded-lg flex items-center justify-center">
                <svelte:component this={item.icon} class="text-blue-600" size={18} />
              </div>
              <span class="text-gray-800 font-medium">{item.label}</span>
            </div>
            
            {#if item.hasToggle}
              <div class="w-12 h-6 bg-blue-500 rounded-full relative">
                <div class="w-5 h-5 bg-white rounded-full absolute top-0.5 right-0.5 transition-transform"></div>
              </div>
            {:else}
              <ChevronRight class="text-gray-400" size={20} />
            {/if}
          </button>
        {/each}
      </div>

      <!-- App Info -->
      <div class="mt-8 pt-6 border-t border-gray-100">
        <div class="text-center space-y-2">
          <div class="w-16 h-16 bg-blue-500 rounded-2xl mx-auto flex items-center justify-center">
            <Heart class="text-white" size={24} />
          </div>
          <h3 class="font-semibold text-gray-800">寵物領養應用程式</h3>
          <p class="text-sm text-gray-500">版本 1.0.0</p>
        </div>
      </div>

      <!-- Logout Button -->
      <div class="mt-8 pb-20">
        <button class="w-full flex items-center justify-center gap-2 py-4 text-red-500 font-medium">
          <LogOut size={20} />
          登出
        </button>
      </div>
    </div>
  {:else if activeTab === 'notifications'}
    <div class="p-4">
      <button 
        on:click={() => setActiveTab('settings')}
        class="mb-4 text-blue-500 flex items-center gap-1 text-sm"
      >
        <ArrowLeft size={16} />
        返回設定
      </button>
      
      <NotificationsComponent />
    </div>
  {:else if activeTab === 'location'}
    <div class="p-4">
      <button 
        on:click={() => setActiveTab('settings')}
        class="mb-4 text-blue-500 flex items-center gap-1 text-sm"
      >
        <ArrowLeft size={16} />
        返回設定
      </button>
      
      <GeolocationComponent />
    </div>
  {:else if activeTab === 'camera'}
    <div class="p-4">
      <button 
        on:click={() => setActiveTab('settings')}
        class="mb-4 text-blue-500 flex items-center gap-1 text-sm"
      >
        <ArrowLeft size={16} />
        返回設定
      </button>
      
      <CameraComponent />
    </div>
  {/if}
</div>
