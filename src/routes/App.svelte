<script>
  import { writable } from 'svelte/store';
  import PetListings from './PetListings.svelte';
  import PetProfile from './PetProfile.svelte';
  import AdoptionForm from './AdoptionForm.svelte';
  import Settings from './Settings.svelte';
  import UserProfile from './UserProfile.svelte';

  let currentPage = $state('listings');
  let selectedPet = $state(null);

  function navigateTo(page, pet = null) {
    currentPage = page;
    selectedPet = pet;
  }
</script>

<div class="min-h-screen bg-gray-50">
  {#if currentPage === 'listings'}
    <PetListings {navigateTo} />
  {:else if currentPage === 'profile'}
    <PetProfile pet={selectedPet} {navigateTo} />
  {:else if currentPage === 'adoption'}
    <AdoptionForm pet={selectedPet} {navigateTo} />
  {:else if currentPage === 'settings'}
    <Settings {navigateTo} />
  {:else if currentPage === 'user'}
    <UserProfile {navigateTo} />
  {/if}
</div>
