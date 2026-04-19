<script lang="ts">
  import { fade, fly } from 'svelte/transition';
  import type { vehicule } from '../type';
  import BookingButton from './BookingButton.svelte';

  let { vehicules = [] } = $props<{ vehicules: vehicule[] }>();
  let currentIndex = $state(0);

  function next() { currentIndex = (currentIndex + 1) % vehicules.length; }
  function prev() { currentIndex = (currentIndex - 1 + vehicules.length) % vehicules.length; }

  let current = $derived(vehicules[currentIndex]);
</script>

<section class="bg-[#F8F8F5] py-32 px-6 md:px-12 overflow-hidden border-t border-slate-100">
  <div class="max-w-[1800px] mx-auto">
    
    <!-- Header Navigation -->
    <div class="flex justify-between items-end mb-24 border-b border-slate-200 pb-12">      
      <div class="flex items-center gap-12">
        <div class="flex gap-2">
          <button on:click={prev} class="group w-14 h-14 flex items-center justify-center border border-slate-200 rounded-full hover:bg-black transition-all duration-500">
            <span class="group-hover:text-white transition-colors text-2xl font-light">←</span>
          </button>
          <button on:click={next} class="group w-14 h-14 flex items-center justify-center border border-slate-200 rounded-full hover:bg-black transition-all duration-500">
            <span class="group-hover:text-white transition-colors text-2xl font-light">→</span>
          </button>
        </div>
        <div class="h-14 w-[1px] bg-slate-200 hidden md:block"></div>
        <div class="hidden md:flex flex-col items-end">
          <span class="text-3xl font-serif text-slate-900 leading-none">{String(currentIndex + 1).padStart(2, '0')}</span>
          <span class="text-[10px] uppercase tracking-widest text-slate-400 font-bold mt-1">out of {vehicules.length}</span>
        </div>
      </div>
    </div>

    {#if current}
      <div class="grid grid-cols-1 lg:grid-cols-12 gap-16 lg:gap-24 items-start lg:items-stretch">
        
        <!-- Left: Information (5 cols) - Sticky -->
        {#key currentIndex}
        <div in:fly={{ x: -30, duration: 800, delay: 200 }} out:fade={{ duration: 400 }} class="lg:col-span-5 flex flex-col lg:sticky lg:top-40 h-fit">
          <span class="text-[10px] tracking-[0.8em] text-slate-400 uppercase font-bold mb-10 block">
            Automotive Heritage — Arch. {current.id || '00' + (currentIndex + 1)}
          </span>
          
          <h3 class="text-7xl md:text-[9rem] font-serif mb-12 text-slate-900 tracking-tighter leading-[0.85] italic">
            {current.model}
          </h3>

          <div class="border-l-2 border-slate-900 pl-10 mb-16">
            <p class="text-2xl md:text-3xl text-slate-600 leading-relaxed font-light italic max-w-lg">
              {current.description}
            </p>
          </div>

          <div class="flex flex-col gap-10">
            <div class="flex justify-between items-end border-b border-slate-200 pb-8">
              <span class="text-[10px] uppercase tracking-[0.5em] text-slate-400 font-bold">Estimated Value</span>
              <span class="text-5xl font-serif text-slate-900 tracking-tighter">{current.price.toLocaleString()}€</span>
            </div>
            
            <div class="flex items-center gap-6">
              <div class="flex-grow">
                <BookingButton />
              </div>
              <p class="text-[10px] uppercase tracking-widest text-slate-400 max-w-[120px] leading-relaxed">
                Full appraisal available upon request.
              </p>
            </div>
          </div>
        </div>
        {/key}

        <!-- Right: Scrollable Gallery (7 cols) -->
        <div class="lg:col-span-7 relative min-h-[500px] lg:min-h-0">
          {#if current.images && current.images.length > 0}
            <div class="lg:absolute lg:inset-0 lg:overflow-y-auto flex flex-col gap-12 lg:pr-6 custom-scrollbar">
              {#each current.images as image, i}
                {#key currentIndex}
                  <div 
                    in:fly={{ y: 40, delay: i * 150, duration: 1000 }}
                    class="w-full bg-white shadow-sm border border-slate-100"
                  >
                    <img 
                      src={image.url} 
                      alt={`${current.model} - View ${i + 1}`}
                      class="w-full h-auto block" 
                      loading="lazy"
                    />
                  </div>
                {/key}
              {/each}
            </div>
          {:else}
            <div class="lg:absolute lg:inset-0 aspect-video w-full flex flex-col items-center justify-center bg-white border border-slate-100 italic text-slate-400 uppercase tracking-widest text-xs">
              Photographic Documentation in progress...
            </div>
          {/if}
        </div>

      </div>
    {/if}
  </div>
</section>

<style>
  :global(.font-serif) {
    font-family: 'Instrument Serif', serif;
  }

  .custom-scrollbar::-webkit-scrollbar {
    width: 4px;
  }
  .custom-scrollbar::-webkit-scrollbar-track {
    background: transparent;
  }
  .custom-scrollbar::-webkit-scrollbar-thumb {
    background: #cbd5e1;
    border-radius: 10px;
  }
  .custom-scrollbar::-webkit-scrollbar-thumb:hover {
    background: #94a3b8;
  }
</style>