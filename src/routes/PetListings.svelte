<script>
  import { Heart, Plus, Search, Filter } from 'lucide-svelte';
  
  let { navigateTo } = $props();

  const pets = [
    {
      id: 1,
      name: 'Luna',
      type: 'Cat',
      age: '2 years',
      ageNumber: 2,
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
      ageNumber: 3,
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
      ageNumber: 1,
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
      ageNumber: 4,
      breed: 'Labrador',
      location: 'Kaohsiung Shelter',
      image: '/placeholder.svg?height=80&width=80',
      urgent: false
    }
  ];

  let searchQuery = $state('');
  let selectedType = $state('all');
  let selectedAge = $state('all');

  let filteredPets = $derived(pets.filter(pet => {
    const matchesSearch = pet.name.toLowerCase().includes(searchQuery.toLowerCase()) ||
                         pet.breed.toLowerCase().includes(searchQuery.toLowerCase()) ||
                         pet.location.toLowerCase().includes(searchQuery.toLowerCase());
    
    const matchesType = selectedType === 'all' || pet.type.toLowerCase() === selectedType;
    
    const matchesAge = selectedAge === 'all' || 
                      (selectedAge === 'young' && pet.ageNumber <= 2) ||
                      (selectedAge === 'adult' && pet.ageNumber > 2 && pet.ageNumber <= 5) ||
                      (selectedAge === 'senior' && pet.ageNumber > 5);
    
    return matchesSearch && matchesType && matchesAge;
  }));
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
    <div class="flex gap-4 mb-4">
      <button 
        class="px-4 py-2 rounded-full text-sm font-medium transition-colors"
        class:bg-blue-100={selectedType === 'all'}
        class:text-blue-600={selectedType === 'all'}
        class:bg-gray-100={selectedType !== 'all'}
        class:text-gray-600={selectedType !== 'all'}
        onclick={() => selectedType = 'all'}
      >
        All Pets
      </button>
      <button 
        class="px-4 py-2 rounded-full text-sm font-medium transition-colors"
        class:bg-blue-100={selectedType === 'dog'}
        class:text-blue-600={selectedType === 'dog'}
        class:bg-gray-100={selectedType !== 'dog'}
        class:text-gray-600={selectedType !== 'dog'}
        onclick={() => selectedType = 'dog'}
      >
        Dogs
      </button>
      <button 
        class="px-4 py-2 rounded-full text-sm font-medium transition-colors"
        class:bg-blue-100={selectedType === 'cat'}
        class:text-blue-600={selectedType === 'cat'}
        class:bg-gray-100={selectedType !== 'cat'}
        class:text-gray-600={selectedType !== 'cat'}
        onclick={() => selectedType = 'cat'}
      >
        Cats
      </button>
    </div>
    
    <!-- Age Filter -->
    <div class="flex gap-2">
      <span class="text-sm font-medium text-gray-700 self-center mr-2">Age:</span>
      <button 
        class="px-3 py-1 rounded-full text-xs font-medium transition-colors"
        class:bg-green-100={selectedAge === 'all'}
        class:text-green-600={selectedAge === 'all'}
        class:bg-gray-100={selectedAge !== 'all'}
        class:text-gray-600={selectedAge !== 'all'}
        onclick={() => selectedAge = 'all'}
      >
        All
      </button>
      <button 
        class="px-3 py-1 rounded-full text-xs font-medium transition-colors"
        class:bg-green-100={selectedAge === 'young'}
        class:text-green-600={selectedAge === 'young'}
        class:bg-gray-100={selectedAge !== 'young'}
        class:text-gray-600={selectedAge !== 'young'}
        onclick={() => selectedAge = 'young'}
      >
        Young (0-2)
      </button>
      <button 
        class="px-3 py-1 rounded-full text-xs font-medium transition-colors"
        class:bg-green-100={selectedAge === 'adult'}
        class:text-green-600={selectedAge === 'adult'}
        class:bg-gray-100={selectedAge !== 'adult'}
        class:text-gray-600={selectedAge !== 'adult'}
        onclick={() => selectedAge = 'adult'}
      >
        Adult (3-5)
      </button>
      <button 
        class="px-3 py-1 rounded-full text-xs font-medium transition-colors"
        class:bg-green-100={selectedAge === 'senior'}
        class:text-green-600={selectedAge === 'senior'}
        class:bg-gray-100={selectedAge !== 'senior'}
        class:text-gray-600={selectedAge !== 'senior'}
        onclick={() => selectedAge = 'senior'}
      >
        Senior (6+)
      </button>
    </div>
  </div>

  <!-- Pet Listings -->
  <div class="px-4 py-2">
    {#each filteredPets as pet}
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
