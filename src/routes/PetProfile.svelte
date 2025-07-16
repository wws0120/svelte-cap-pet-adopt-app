<script>
  import { ArrowLeft, Heart, Share, MapPin, Calendar, Info } from 'lucide-svelte';
  
  let { pet, navigateTo } = $props();
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
    <h1 class="text-lg font-semibold text-gray-800">Pet Profile</h1>
    <div class="flex gap-2">
      <button>
        <Heart class="text-gray-400" size={24} />
      </button>
      <button>
        <Share class="text-gray-400" size={24} />
      </button>
    </div>
  </div>

  <!-- Pet Image -->
  <div class="px-4 py-6 bg-gray-50">
    <div class="relative mx-auto w-48 h-48">
      <img 
        src={pet?.image || '/placeholder.svg?height=200&width=200'} 
        alt={pet?.name}
        class="w-full h-full rounded-2xl object-cover"
      />
      {#if pet?.urgent}
        <div class="absolute top-3 right-3 bg-red-500 text-white px-2 py-1 rounded-full text-xs font-medium">
          Urgent
        </div>
      {/if}
    </div>
  </div>

  <!-- Pet Info -->
  <div class="px-4 py-4">
    <div class="text-center mb-6">
      <h2 class="text-2xl font-bold text-gray-800 mb-1">{pet?.name || 'Pet Name'}</h2>
      <p class="text-blue-500 font-medium">{pet?.breed || 'Breed'}</p>
      <div class="flex items-center justify-center gap-1 mt-2 text-gray-500">
        <MapPin size={16} />
        <span class="text-sm">{pet?.location || 'Location'}</span>
      </div>
    </div>

    <!-- Pet Details -->
    <div class="space-y-4 mb-6">
      <div class="flex justify-between items-center py-3 border-b border-gray-100">
        <span class="text-gray-600">Age</span>
        <span class="font-medium text-gray-800">{pet?.age || '2 years'}</span>
      </div>
      <div class="flex justify-between items-center py-3 border-b border-gray-100">
        <span class="text-gray-600">Type</span>
        <span class="font-medium text-gray-800">{pet?.type || 'Dog'}</span>
      </div>
      <div class="flex justify-between items-center py-3 border-b border-gray-100">
        <span class="text-gray-600">Gender</span>
        <span class="font-medium text-gray-800">Male</span>
      </div>
      <div class="flex justify-between items-center py-3 border-b border-gray-100">
        <span class="text-gray-600">Vaccination</span>
        <span class="font-medium text-green-600">Up to date</span>
      </div>
    </div>

    <!-- Description -->
    <div class="mb-6">
      <h3 class="font-semibold text-gray-800 mb-2">About {pet?.name}</h3>
      <p class="text-gray-600 text-sm leading-relaxed">
        {pet?.name} is a friendly and energetic {pet?.type?.toLowerCase()} looking for a loving home. 
        Great with children and other pets. Fully vaccinated and ready for adoption.
      </p>
    </div>

    <!-- Action Buttons -->
    <div class="space-y-3 pb-20">
      <button 
        onclick={() => navigateTo('adoption', pet)}
        class="w-full bg-blue-500 text-white py-4 rounded-lg font-semibold"
      >
        Adopt {pet?.name}
      </button>
      <button class="w-full border border-blue-500 text-blue-500 py-4 rounded-lg font-semibold">
        Contact Shelter
      </button>
    </div>
  </div>
</div>
