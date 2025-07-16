# 寵物領養應用程式 - 技術文檔

## 專案概述

這是一個使用 SvelteKit 2 和 Svelte 5 構建的現代化寵物領養應用程式，旨在幫助用戶瀏覽、搜尋和領養來自各個動物收容所的寵物。該應用程式提供移動優先的用戶界面，具備寵物列表瀏覽、詳細寵物檔案、領養申請、用戶檔案和設定等功能。應用程式可以使用 Capacitor 打包為原生移動應用程式，支援 iOS 和 Android 平台。

## 技術架構

### 核心技術
- **Svelte 5**: 前端框架，使用新的 runes API 進行響應式狀態管理
- **SvelteKit 2**: 用於構建 Svelte 應用程式的全端框架
- **TypeScript**: 提供型別安全和改善開發體驗
- **Vite**: 建置工具和開發伺服器
- **Capacitor**: 用於將 Web 應用程式轉換為原生移動應用程式的跨平台運行時

### UI 和樣式
- **TailwindCSS 4**: 實用優先的 CSS 框架
- **Lucide Icons**: 適用於 Svelte 的 SVG 圖示庫

## 專案結構

專案遵循標準的 SvelteKit 結構，並整合了 Capacitor：

```
my-project/
├ src/
│ ├ routes/
│ │ ├ +page.svelte         # 主要入口點
│ │ ├ App.svelte           # 主應用程式元件
│ │ ├ PetListings.svelte   # 寵物列表頁面
│ │ ├ PetProfile.svelte    # 個別寵物檔案頁面
│ │ ├ AdoptionForm.svelte  # 寵物領養表單
│ │ ├ UserProfile.svelte   # 用戶檔案頁面
│ │ ├ Settings.svelte      # 應用程式設定
│ │ └ +layout.svelte       # 佈局元件，包含 CSS 匯入
│ ├ app.html               # HTML 模板
│ ├ app.css                # 全域 CSS，包含 TailwindCSS 匯入
│ └ app.d.ts               # TypeScript 宣告
├ static/                  # 靜態資源
├ capacitor.config.ts      # Capacitor 配置
├ android/                 # Android 平台特定代碼
├ ios/                     # iOS 平台特定代碼
├ package.json             # 依賴項目和腳本
├ svelte.config.js         # Svelte 配置
├ vite.config.ts           # Vite 配置
└ tsconfig.json            # TypeScript 配置
```

## 功能特色

### 寵物列表
- 顯示可領養寵物的基本資訊
- 搜尋功能以篩選寵物
- 緊急領養指示器
- 導航至詳細寵物檔案

### 寵物檔案
- 每隻寵物的詳細資訊
- 疫苗接種和健康狀態
- 領養按鈕以開始流程
- 聯絡收容所選項

### 領養表單
- 寵物領養申請表單
- 個人資訊收集
- 領養原因和經驗問題

### 用戶檔案
- 用戶資訊顯示
- 領養歷史記錄
- 最愛寵物清單
- 檔案編輯功能

### 設定
- 通知偏好設定
- 位置設定
- 隱私和安全選項
- 帳戶管理

### 移動應用程式特定功能
- 推送通知
- 相機存取（用於上傳寵物照片）
- 地理位置服務（尋找附近收容所）
- 離線模式支援
- 生物識別認證（指紋/臉部辨識）

## 實作細節

### 狀態管理
應用程式使用 Svelte 5 的新 runes API 進行狀態管理：
- `$state` 用於響應式本地狀態
- `$props()` 用於元件屬性

### 導航
頁面間的導航透過自定義導航函數處理：
```javascript
function navigateTo(page, pet = null) {
  currentPage = page;
  selectedPet = pet;
}
```

### 響應式設計
UI 採用移動優先的設計方法，使用 TailwindCSS 實用類別。

### 資料結構
寵物資料包含以下欄位：
- id: 唯一識別碼
- name: 寵物名稱
- type: 寵物類型（狗、貓等）
- age: 年齡
- breed: 品種
- location: 收容所位置
- image: 寵物照片
- urgent: 是否為緊急領養

## 開發設定

1. 複製專案儲存庫
2. 安裝依賴項目：
   ```bash
   npm install
   ```
3. 安裝 Capacitor：
   ```bash
   npm install @capacitor/core @capacitor/cli
   npx cap init
   ```
4. 啟動開發伺服器：
   ```bash
   npm run dev
   ```
5. 建置正式版本：
   ```bash
   npm run build
   ```
6. 新增移動平台：
   ```bash
   npm install @capacitor/ios @capacitor/android
   npx cap add ios
   npx cap add android
   ```
7. 同步 Web 代碼到原生專案：
   ```bash
   npx cap sync
   ```
8. 開啟原生 IDE 進行開發：
   ```bash
   npx cap open ios     # 開啟 Xcode
   npx cap open android # 開啟 Android Studio
   ```

## Capacitor 配置

在專案根目錄創建 `capacitor.config.ts` 文件：

```typescript
import { CapacitorConfig } from '@capacitor/cli';

const config: CapacitorConfig = {
  appId: 'com.petadoption.app',
  appName: '寵物領養',
  webDir: 'build',
  bundledWebRuntime: false,
  plugins: {
    SplashScreen: {
      launchShowDuration: 2000,
      backgroundColor: "#3B82F6",
      androidSplashResourceName: "splash",
      androidScaleType: "CENTER_CROP",
      showSpinner: true,
      spinnerColor: "#FFFFFF",
      splashFullScreen: true,
      splashImmersive: true
    },
    PushNotifications: {
      presentationOptions: ["badge", "sound", "alert"]
    }
  }
};

export default config;
```

## SvelteKit 適配器配置

為了與 Capacitor 正確整合，需要修改 SvelteKit 配置以使用靜態適配器：

1. 安裝靜態適配器：
   ```bash
   npm install @sveltejs/adapter-static
   ```

2. 更新 `svelte.config.js`：
   ```javascript
   import adapter from '@sveltejs/adapter-static';
   import { vitePreprocess } from '@sveltejs/vite-plugin-svelte';

   /** @type {import('@sveltejs/kit').Config} */
   const config = {
     preprocess: vitePreprocess(),

     kit: {
       adapter: adapter({
         pages: 'build',
         assets: 'build',
         fallback: 'index.html',
         precompress: false
       })
     }
   };

   export default config;
   ```

## 部署

### Web 部署
應用程式使用 `@sveltejs/adapter-static` 生成靜態網站，可部署到任何靜態網站託管服務。

### 移動應用程式部署
1. 使用 Capacitor 構建原生應用程式
2. 透過 App Store (iOS) 和 Google Play Store (Android) 發布應用程式

## 原生功能整合

### 相機存取
使用 Capacitor Camera API 實現照片上傳功能：

```javascript
import { Camera, CameraResultType } from '@capacitor/camera';

async function takePicture() {
  const image = await Camera.getPhoto({
    quality: 90,
    allowEditing: true,
    resultType: CameraResultType.Uri
  });
  
  // 處理照片上傳邏輯
}
```

### 推送通知
使用 Capacitor Push Notifications API 實現通知功能：

```javascript
import { PushNotifications } from '@capacitor/push-notifications';

// 註冊推送通知
async function registerPushNotifications() {
  await PushNotifications.requestPermissions();
  await PushNotifications.register();
}

// 監聽推送通知
PushNotifications.addListener('pushNotificationReceived', (notification) => {
  // 處理收到的通知
});
```

### 地理位置
使用 Capacitor Geolocation API 實現位置功能：

```javascript
import { Geolocation } from '@capacitor/geolocation';

async function getCurrentPosition() {
  const position = await Geolocation.getCurrentPosition();
  const { latitude, longitude } = position.coords;
  
  // 使用位置資訊尋找附近收容所
}
```

## 未來增強功能

潛在的增強領域包括：
- 與真實資料庫的後端整合
- 身份驗證系統
- 地理位置功能以尋找附近的收容所
- 寵物可領養通知
- 收容所管理的管理員儀表板
- 多語言支援
- 寵物搜尋篩選器（年齡、大小、品種等）
- 社交分享功能
- 寵物照片上傳功能
- 即時聊天支援
- 離線模式資料同步
- 應用內購買（捐款功能）
- AR 功能（預覽寵物在家中的樣子）

## 技術特點

### 效能優化
- 使用 Vite 進行快速建置和熱重載
- SvelteKit 的自動程式碼分割
- 響應式圖片載入
- 原生移動應用程式效能

### 可訪問性
- 語義化 HTML 結構
- 鍵盤導航支援
- 適當的 ARIA 標籤
- 支援移動裝置輔助功能

### 開發體驗
- TypeScript 支援完整的型別檢查
- Prettier 程式碼格式化
- ESLint 程式碼品質檢查
- 統一的 Web 和移動應用程式開發流程
