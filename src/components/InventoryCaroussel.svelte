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

<section class="bg-[#F5F5F0] py-24 px-8 md:px-16 min-h-[800px] overflow-hidden">
  <div class="max-w-7xl mx-auto">
    
    <div class="flex justify-between items-center mb-16">
      <h2 class="text-4xl font-serif tracking-tight text-slate-900 italic">La Sélection</h2>
      {#if vehicules.length > 1}
      <div class="flex gap-4">
        <button on:click={prev} class="w-12 h-12 border border-slate-200 rounded-full hover:bg-black hover:text-white transition-all duration-500 font-light text-xl">←</button>
        <button on:click={next} class="w-12 h-12 border border-slate-200 rounded-full hover:bg-black hover:text-white transition-all duration-500 font-light text-xl">→</button>
      </div>
      {/if}

      <div class="flex items-center gap-8">
        <BookingButton />
        <span class="text-[10px] tracking-[0.3em] text-slate-400 uppercase font-bold">
          {currentIndex + 1} / {vehicules.length}
        </span>
      </div>
    </div>

    {#if current}
      <div class="grid grid-cols-1 lg:grid-cols-2 gap-20 items-start">
        
        {#key currentIndex}
        <div in:fly={{ x: -20, duration: 600 }} out:fade={{ duration: 300 }} class="flex flex-col h-full sticky top-24">
          <span class="text-[10px] tracking-[0.6em] text-slate-400 uppercase font-bold mb-8 block">
            Archives — No.{String(currentIndex + 1).padStart(3, '0')}
          </span>
          
          <h3 class="text-7xl md:text-8xl font-serif mb-10 text-slate-900 tracking-tight leading-[0.9] italic">
            {current.model}
          </h3>

          <div class="max-w-md mb-12 pr-10 h-[350px] overflow-y-auto custom-scrollbar border-l border-slate-100 pl-8">
            <p class="text-slate-500 text-xl leading-relaxed font-light italic">
              {current.description}
            </p>
          </div>

          <div class="text-6xl font-serif text-slate-900 mt-auto mb-10 border-b border-slate-100 pb-10 w-full flex justify-between items-baseline">
            <span class="text-[10px] uppercase tracking-[0.4em] text-slate-400 font-bold">Investissement</span>
            <span>{current.price.toLocaleString()}€</span>
          </div>

        </div>
        {/key}

        <div class="h-[650px] overflow-y-auto pr-4 custom-scrollbar">
          {#if current.images && current.images.length > 0}
            <div class="columns-1 md:columns-2 gap-4 space-y-4">
              {#each current.images as image, i}
                {#key currentIndex}
                  <div 
                    in:fly={{ y: 20, delay: i * 50, duration: 800 }}
                    class="relative overflow-hidden rounded-2xl bg-white shadow-sm border border-slate-100 hover:shadow-xl hover:-translate-y-1 transition-all duration-700 group break-inside-avoid"
                  >
                    <div class="flex items-center justify-center p-4 min-h-[150px]">
                      <img 
                        src={image.url} 
                        alt={`${current.model} - View ${i + 1}`}
                        class="w-full h-auto object-contain drop-shadow-[0_10px_15px_rgba(0,0,0,0.1)] group-hover:scale-105 group-hover:drop-shadow-[0_20px_25px_rgba(0,0,0,0.2)] transition-all duration-700"
                      />
                    </div>
                    <div class="absolute bottom-2 right-2 text-[7px] uppercase tracking-tight text-slate-300 opacity-0 group-hover:opacity-100 transition-opacity">
                      Shot_{String(i + 1).padStart(3, '0')}
                    </div>
                  </div>
                {/key}
              {/each}
            </div>
          {:else}
            <div class="w-full h-full flex flex-col items-center justify-center bg-white rounded-2xl border border-dashed border-slate-200 text-center p-12">
              <span class="text-4xl mb-4 opacity-50">📸</span>
              <p class="text-xs uppercase text-slate-400 tracking-widest italic">No Archive Photos Available</p>
            </div>
          {/if}
        </div>

      </div>
    {/if}
  </div>
</section>

<style>
  .custom-scrollbar::-webkit-scrollbar { width: 3px; }
  .custom-scrollbar::-webkit-scrollbar-track { background: transparent; }
  .custom-scrollbar::-webkit-scrollbar-thumb { background: #cbd5e1; border-radius: 10px; }
  .custom-scrollbar::-webkit-scrollbar-thumb:hover { background: #94a3b8; }
  
  .custom-scrollbar { scrollbar-width: thin; scrollbar-color: #cbd5e1 transparent; }
</style>