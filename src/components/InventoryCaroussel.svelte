<script lang="ts">
  import { fade, fly } from 'svelte/transition';
  import type { vehicule } from '../type';

  let { vehicules = [] } = $props<{ vehicules: vehicule[] }>();
  let currentIndex = $state(0);

  function next() { currentIndex = (currentIndex + 1) % vehicules.length; }
  function prev() { currentIndex = (currentIndex - 1 + vehicules.length) % vehicules.length; }

  let current = $derived(vehicules[currentIndex]);
</script>

<section class="bg-[#F5F5F0] py-24 px-8 md:px-16 min-h-[800px] overflow-hidden">
  <div class="max-w-7xl mx-auto">
    
    <div class="flex justify-between items-center mb-16">
      <h2 class="text-3xl font-serif tracking-widest uppercase text-slate-900">Featured Archive</h2>
      <div class="flex gap-4">
        <button on:click={prev} class="w-12 h-12 border border-slate-300 rounded-full hover:bg-black hover:text-white transition-all duration-300">←</button>
        <button on:click={next} class="w-12 h-12 border border-slate-300 rounded-full hover:bg-black hover:text-white transition-all duration-300">→</button>
      </div>
    </div>

    {#if current}
      <div class="grid grid-cols-1 lg:grid-cols-2 gap-16 items-start">
        
        {#key currentIndex}
        <div in:fly={{ x: -20, duration: 600 }} out:fade={{ duration: 300 }} class="flex flex-col h-full sticky top-24">
          <span class="text-[10px] tracking-[0.4em] text-indigo-600 uppercase font-bold mb-4 block">
            Ref. 00{currentIndex + 1}
          </span>
          
          <h3 class="text-6xl font-serif mb-6 text-slate-900 uppercase tracking-tighter leading-none">
            {current.model}
          </h3>

          <div class="max-w-md mb-8 pr-4 h-48 overflow-y-auto custom-scrollbar">
            <p class="text-slate-600 text-lg leading-relaxed italic font-light">
              {current.description}
            </p>
          </div>

          <div class="text-4xl font-light text-slate-900 mb-10 border-b border-slate-300 pb-6 w-full">
            {current.price.toLocaleString()} €
          </div>

        </div>
        {/key}

        <div class="h-[650px] overflow-y-auto pr-4 custom-scrollbar">
          {#if current.images && current.images.length > 0}
            <div class="columns-2 gap-4 space-y-4">
              {#each current.images as image, i}
                {#key currentIndex}
                  <div 
                    in:fly={{ y: 20, delay: i * 50, duration: 800 }}
                    class="relative overflow-hidden rounded-2xl bg-white shadow-sm border border-slate-100 hover:shadow-xl hover:-translate-y-1 transition-all duration-700 group break-inside-avoid"
                  >
                    <div class="flex items-center justify-center p-4 min-h-[150px]">
                      <img 
                        src={image} 
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