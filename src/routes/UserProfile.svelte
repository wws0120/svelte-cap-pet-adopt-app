<script>
  import { ArrowLeft, Edit, Heart, Calendar, Award, Settings } from 'lucide-svelte';
  
  let { navigateTo } = $props();

  const adoptionHistory = [
    { name: 'Buddy', date: '2023-08-15', status: 'Adopted' },
    { name: 'Whiskers', date: '2023-06-20', status: 'Adopted' },
  ];

  const favoritesPets = [
    { name: 'Luna', breed: 'Persian Cat', image: '/placeholder.svg?height=60&width=60' },
    { name: 'Rocky', breed: 'German Shepherd', image: '/placeholder.svg?height=60&width=60' },
  ];
</script>

<div class="max-w-md mx-auto bg-white min-h-screen">
  <!-- Status Bar -->
  <div class="flex justify-between items-center px-4 py-2 text-sm bg-white">
    <span class="font-medium">9:41</span>
    <div class="flex items-center gap-1">
      <div class="flex gap-1">
        <div class="w-1 h-3 bg-gray-800 rounded-full"></div>
        <div class="w-1 h-3 bg-gray-800 rounded-full"></div>
        <div class="w-1 h-3 bg-gray-800 rounded-full"></div>
        <div class="w-1 h-3 bg-gray-400 rounded-full"></div>
      </div>
      <span class="ml-2 text-gray-800">100%</span>
      <div class="w-6 h-3 border border-gray-800 rounded-sm ml-1">
        <div class="w-full h-full bg-green-500 rounded-sm"></div>
      </div>
    </div>
  </div>

  <!-- Header -->
  <div class="flex justify-between items-center px-4 py-4 bg-white border-b border-gray-100">
    <button onclick={() => navigateTo('listings')}>
      <ArrowLeft class="text-gray-600" size={24} />
    </button>
    <h1 class="text-lg font-semibold text-gray-800">My Profile</h1>
    <button onclick={() => navigateTo('settings')}>
      <Settings class="text-gray-600" size={24} />
    </button>
  </div>

  <!-- Profile Header -->
  <div class="px-4 py-6 bg-blue-50">
    <div class="text-center">
      <div class="relative inline-block">
        <img 
          src="/placeholder.svg?height=80&width=80" 
          alt="Profile"
          class="w-20 h-20 rounded-full object-cover mx-auto"
        />
        <button class="absolute bottom-0 right-0 w-6 h-6 bg-blue-500 rounded-full flex items-center justify-center">
          <Edit class="text-white" size={14} />
        </button>
      </div>
      <h2 class="text-xl font-bold text-gray-800 mt-3">John Doe</h2>
      <p class="text-gray-600">john.doe@email.com</p>
      <p class="text-sm text-gray-500 mt-1">Pet Lover since 2020</p>
    </div>
  </div>

  <!-- Stats -->
  <div class="px-4 py-4 bg-white border-b border-gray-100">
    <div class="grid grid-cols-3 gap-4">
      <div class="text-center">
        <div class="w-12 h-12 bg-green-100 rounded-full flex items-center justify-center mx-auto mb-2">
          <Award class="text-green-600" size={20} />
        </div>
        <p class="text-2xl font-bold text-gray-800">2</p>
        <p class="text-xs text-gray-500">Adopted</p>
      </div>
      <div class="text-center">
        <div class="w-12 h-12 bg-red-100 rounded-full flex items-center justify-center mx-auto mb-2">
          <Heart class="text-red-600" size={20} />
        </div>
        <p class="text-2xl font-bold text-gray-800">5</p>
        <p class="text-xs text-gray-500">Favorites</p>
      </div>
      <div class="text-center">
        <div class="w-12 h-12 bg-blue-100 rounded-full flex items-center justify-center mx-auto mb-2">
          <Calendar class="text-blue-600" size={20} />
        </div>
        <p class="text-2xl font-bold text-gray-800">3</p>
        <p class="text-xs text-gray-500">Applications</p>
      </div>
    </div>
  </div>

  <!-- Adoption History -->
  <div class="px-4 py-4">
    <h3 class="font-semibold text-gray-800 mb-3">Adoption History</h3>
    <div class="space-y-3">
      {#each adoptionHistory as adoption}
        <div class="flex items-center justify-between p-3 bg-gray-50 rounded-lg">
          <div>
            <p class="font-medium text-gray-800">{adoption.name}</p>
            <p class="text-sm text-gray-500">{adoption.date}</p>
          </div>
          <span class="px-2 py-1 bg-green-100 text-green-600 text-xs rounded-full">
            {adoption.status}
          </span>
        </div>
      {/each}
    </div>
  </div>

  <!-- Favorite Pets -->
  <div class="px-4 py-4">
    <h3 class="font-semibold text-gray-800 mb-3">Favorite Pets</h3>
    <div class="space-y-3 pb-20">
      {#each favoritesPets as pet}
        <div class="flex items-center gap-3 p-3 bg-gray-50 rounded-lg">
          <img 
            src={pet.image || "/placeholder.svg"} 
            alt={pet.name}
            class="w-12 h-12 rounded-full object-cover"
          />
          <div class="flex-1">
            <p class="font-medium text-gray-800">{pet.name}</p>
            <p class="text-sm text-gray-500">{pet.breed}</p>
          </div>
          <button>
            <Heart class="text-red-500 fill-current" size={20} />
          </button>
        </div>
      {/each}
    </div>
  </div>
</div>
