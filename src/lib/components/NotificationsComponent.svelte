<script lang="ts">
  import { onMount } from 'svelte';
  import { PushNotifications } from '@capacitor/push-notifications';
  
  let notificationsEnabled = $state(false);
  let notificationsList = $state([]);
  let isNative = $state(false);
  
  onMount(() => {
    // 檢查是否在原生環境中運行
    isNative = window.Capacitor && window.Capacitor.isNative;
    
    if (isNative) {
      setupPushNotifications();
    }
  });
  
  async function setupPushNotifications() {
    try {
      // 請求權限
      const permissionStatus = await PushNotifications.requestPermissions();
      
      if (permissionStatus.receive === 'granted') {
        notificationsEnabled = true;
        
        // 註冊推送通知
        await PushNotifications.register();
        
        // 設置監聽器
        PushNotifications.addListener('pushNotificationReceived', (notification) => {
          notificationsList = [...notificationsList, {
            id: Date.now().toString(),
            title: notification.title,
            body: notification.body,
            date: new Date().toLocaleString()
          }];
        });
      }
    } catch (error) {
      console.error('推送通知設置錯誤', error);
    }
  }
  
  // 模擬發送本地通知（僅用於演示）
  function sendLocalNotification() {
    if (!isNative) {
      // 在網頁環境中模擬通知
      notificationsList = [...notificationsList, {
        id: Date.now().toString(),
        title: '模擬通知',
        body: '這是一個模擬的本地通知。在原生應用中，您將收到真實的推送通知。',
        date: new Date().toLocaleString()
      }];
    }
  }
</script>

<div class="flex flex-col gap-4 p-4">
  <div class="flex items-center justify-between">
    <h3 class="font-semibold text-gray-800">推送通知</h3>
    
    {#if isNative}
      <div class="flex items-center gap-2">
        <span class="text-sm text-gray-600">狀態:</span>
        <span class={notificationsEnabled ? "text-green-500" : "text-red-500"}>
          {notificationsEnabled ? '已啟用' : '未啟用'}
        </span>
      </div>
    {/if}
  </div>
  
  {#if !isNative}
    <div class="bg-yellow-50 border border-yellow-200 p-3 rounded-lg text-sm text-yellow-800">
      推送通知功能僅在原生移動應用程式中可用。在瀏覽器中，您將看到模擬通知。
    </div>
  {/if}
  
  <button 
    on:click={sendLocalNotification}
    class="px-4 py-2 bg-blue-500 text-white rounded-lg flex items-center justify-center gap-2"
  >
    <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
      <path d="M18 8A6 6 0 0 0 6 8c0 7-3 9-3 9h18s-3-2-3-9"/>
      <path d="M13.73 21a2 2 0 0 1-3.46 0"/>
    </svg>
    {isNative ? '發送測試通知' : '模擬通知'}
  </button>
  
  {#if notificationsList.length > 0}
    <div class="mt-4">
      <h4 class="font-medium text-gray-700 mb-2">通知歷史</h4>
      <div class="space-y-2">
        {#each notificationsList as notification}
          <div class="bg-gray-50 p-3 rounded-lg border border-gray-200">
            <div class="font-medium">{notification.title}</div>
            <div class="text-sm text-gray-600">{notification.body}</div>
            <div class="text-xs text-gray-500 mt-1">{notification.date}</div>
          </div>
        {/each}
      </div>
    </div>
  {/if}
</div>