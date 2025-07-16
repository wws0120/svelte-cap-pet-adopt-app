<script lang="ts">
  import { ArrowLeft, Check } from 'lucide-svelte';
  
  let { pet, navigateTo } = $props();
  
  let formData = $state({
    name: '',
    email: '',
    phone: '',
    address: '',
    experience: '',
    housing: 'apartment',
    hasYard: false,
    otherPets: false,
    reason: ''
  });

  function handleSubmit(event) {
    event.preventDefault();
    // Handle form submission
    alert('Application submitted successfully!');
    navigateTo('listings');
  }
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
    <button onclick={() => navigateTo('profile', pet)}>
      <ArrowLeft class="text-gray-600" size={24} />
    </button>
    <h1 class="text-lg font-semibold text-gray-800">Adoption Application</h1>
    <div class="w-6"></div>
  </div>

  <!-- Pet Summary -->
  <div class="px-4 py-4 bg-blue-50 border-b border-gray-100">
    <div class="flex items-center gap-3">
      <img 
        src={pet?.image || '/placeholder.svg?height=50&width=50'} 
        alt={pet?.name}
        class="w-12 h-12 rounded-full object-cover"
      />
      <div>
        <h3 class="font-semibold text-gray-800">{pet?.name}</h3>
        <p class="text-sm text-gray-600">{pet?.breed} • {pet?.age}</p>
      </div>
    </div>
  </div>

  <!-- Form -->
  <form onsubmit={handleSubmit} class="px-4 py-4 space-y-6">
    <!-- Personal Information -->
    <div>
      <h3 class="font-semibold text-gray-800 mb-4">Personal Information</h3>
      
      <div class="space-y-4">
        <div>
          <label for="name" class="block text-sm font-medium text-gray-700 mb-1">Full Name</label>
          <input
            id="name"
            type="text"
            bind:value={formData.name}
            class="w-full px-3 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
            required
          />
        </div>
        
        <div>
          <label for="email" class="block text-sm font-medium text-gray-700 mb-1">Email</label>
          <input
            id="email"
            type="email"
            bind:value={formData.email}
            class="w-full px-3 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
            required
          />
        </div>
        
        <div>
          <label for="phone" class="block text-sm font-medium text-gray-700 mb-1">Phone Number</label>
          <input
            id="phone"
            type="tel"
            bind:value={formData.phone}
            class="w-full px-3 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
            required
          />
        </div>
      </div>
    </div>

    <!-- Housing Information -->
    <div>
      <h3 class="font-semibold text-gray-800 mb-4">Housing Information</h3>
      
      <div class="space-y-4">
        <div>
          <label for="housing" class="block text-sm font-medium text-gray-700 mb-1">Housing Type</label>
          <select
            id="housing"
            bind:value={formData.housing}
            class="w-full px-3 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
          >
            <option value="apartment">Apartment</option>
            <option value="house">House</option>
            <option value="condo">Condo</option>
          </select>
        </div>
        
        <div class="flex items-center">
          <input
            type="checkbox"
            id="hasYard"
            bind:checked={formData.hasYard}
            class="w-4 h-4 text-blue-600 border-gray-300 rounded focus:ring-blue-500"
          />
          <label for="hasYard" class="ml-2 text-sm text-gray-700">I have a yard</label>
        </div>
        
        <div class="flex items-center">
          <input
            type="checkbox"
            id="otherPets"
            bind:checked={formData.otherPets}
            class="w-4 h-4 text-blue-600 border-gray-300 rounded focus:ring-blue-500"
          />
          <label for="otherPets" class="ml-2 text-sm text-gray-700">I have other pets</label>
        </div>
      </div>
    </div>

    <!-- Experience -->
    <div>
      <label for="experience" class="block text-sm font-medium text-gray-700 mb-1">Pet Experience</label>
      <textarea
        id="experience"
        bind:value={formData.experience}
        rows="3"
        class="w-full px-3 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
        placeholder="Tell us about your experience with pets..."
      ></textarea>
    </div>

    <!-- Reason for Adoption -->
    <div>
      <label for="reason" class="block text-sm font-medium text-gray-700 mb-1">Why do you want to adopt {pet?.name}?</label>
      <textarea
        id="reason"
        bind:value={formData.reason}
        rows="3"
        class="w-full px-3 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
        placeholder="Tell us why you want to adopt this pet..."
        required
      ></textarea>
    </div>

    <!-- Submit Button -->
    <div class="pt-4 pb-20">
      <button
        type="submit"
        class="w-full bg-blue-500 text-white py-4 rounded-lg font-semibold flex items-center justify-center gap-2"
      >
        <Check size={20} />
        Submit Application
      </button>
    </div>
  </form>
</div>
