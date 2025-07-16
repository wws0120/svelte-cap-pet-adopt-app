<script>
  import { Heart, Plus, Search, Filter } from 'lucide-svelte';
  
  let { navigateTo } = $props();

  const pets = [
    {
      id: 1,
      name: 'Luna',
      type: 'Cat',
      age: '2 years',
      breed: 'Persian',
      location: 'Taipei Animal Shelter',
      image: '/placeholder.svg?height=80&width=80',
      urgent: false
    },
    {
      id: 2,
      name: 'Max',
      type: 'Dog',
      age: '3 years',
      breed: 'Golden Retriever',
      location: 'New Taipei Rescue',
      image: '/placeholder.svg?height=80&width=80',
      urgent: true
    },
    {
      id: 3,
      name: 'Mimi',
      type: 'Cat',
      age: '1 year',
      breed: 'British Shorthair',
      location: 'Taichung Pet Center',
      image: '/placeholder.svg?height=80&width=80',
      urgent: false
    },
    {
      id: 4,
      name: 'Buddy',
      type: 'Dog',
      age: '4 years',
      breed: 'Labrador',
      location: 'Kaohsiung Shelter',
      image: '/placeholder.svg?height=80&width=80',
      urgent: false
    }
  ];

  let searchQuery = '';
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
  <div class="bg-blue-500 text-white px-4 py-4">
    <div class="flex justify-between items-center mb-4">
      <h1 class="text-xl font-semibold">Pet Adoption</h1>
      <button 
        onclick={() => navigateTo('user')}
        class="w-8 h-8 bg-white/20 rounded-full flex items-center justify-center"
      >
        <Plus size={20} />
      </button>
    </div>
    
    <!-- Search Bar -->
    <div class="relative">
      <Search class="absolute left-3 top-1/2 transform -translate-y-1/2 text-gray-400" size={16} />
      <input
        type="text"
        placeholder="Search pets..."
        bind:value={searchQuery}
        class="w-full pl-10 pr-4 py-2 rounded-lg bg-white text-gray-800 placeholder-gray-500"
      />
    </div>
  </div>

  <!-- Pet Categories -->
  <div class="px-4 py-4 bg-white border-b border-gray-100">
    <div class="flex gap-4">
      <button class="px-4 py-2 bg-blue-100 text-blue-600 rounded-full text-sm font-medium">
        All Pets
      </button>
      <button class="px-4 py-2 bg-gray-100 text-gray-600 rounded-full text-sm font-medium">
        Dogs
      </button>
      <button class="px-4 py-2 bg-gray-100 text-gray-600 rounded-full text-sm font-medium">
        Cats
      </button>
    </div>
  </div>

  <!-- Pet Listings -->
  <div class="px-4 py-2">
    {#each pets as pet}
      <div 
        role="button" 
        tabindex="0" 
        class="bg-white rounded-lg border border-gray-200 p-4 mb-3 shadow-sm cursor-pointer"
        onclick={() => navigateTo('profile', pet)}
        onkeydown={(e) => { if (e.key === 'Enter') navigateTo('profile', pet); }}
      >
        <div class="flex items-center gap-3">
          <div class="relative">
            <img 
              src={pet.image || "/placeholder.svg"} 
              alt={pet.name}
              class="w-16 h-16 rounded-full object-cover"
            />
          </div>
          <div>
            <h2 class="text-lg font-semibold">{pet.name}</h2>
            <p class="text-sm text-gray-600">Type: {pet.type}</p>
            <p class="text-sm text-gray-600">Age: {pet.age}</p>
            <p class="text-sm text-gray-600">Breed: {pet.breed}</p>
            <p class="text-sm text-gray-600">Location: {pet.location}</p>
            {#if pet.urgent}
              <div class="bg-red-100 text-red-600 px-2 py-1 rounded-full text-xs font-medium mt-2">
                Urgent
              </div>
            {/if}
          </div>
        </div>
      </div>
    {/each}
  </div>
</div>
