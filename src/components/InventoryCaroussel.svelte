<script lang="ts">
  import { fade, fly } from 'svelte/transition';
  import type { vehicule } from '../type';

  let { vehicules = [] } = $props<{ vehicules: vehicule[] }>();

  let currentIndex = $state(0);

  function next() {
    currentIndex = (currentIndex + 1) % vehicules.length;
  }

  function prev() {
    currentIndex = (currentIndex - 1 + vehicules.length) % vehicules.length;
  }

  let current = $derived(vehicules[currentIndex]);
</script>

<section class="bg-[#F5F5F0] py-24 px-8 md:px-16 min-h-[700px] overflow-hidden">
  <div class="max-w-7xl mx-auto">
    
    <div class="flex justify-between items-center mb-16">
      <h2 class="text-3xl font-serif tracking-widest uppercase text-slate-900">Featured Archive</h2>
      <div class="flex gap-4">
        <button on:click={prev} class="w-12 h-12 border border-slate-300 rounded-full hover:bg-black hover:text-white transition-all">←</button>
        <button on:click={next} class="w-12 h-12 border border-slate-300 rounded-full hover:bg-black hover:text-white transition-all">→</button>
      </div>
    </div>

    {#if current}
      <div class="grid grid-cols-1 lg:grid-cols-2 gap-16 items-center">
        
        {#key currentIndex}
        <div in:fly={{ x: -20, duration: 600 }} out:fade={{ duration: 300 }}>
          <span class="text-[10px] tracking-[0.4em] text-indigo-600 uppercase font-bold mb-4 block">Ref. 00{currentIndex + 1}</span>
          <h3 class="text-6xl font-serif mb-6 text-slate-900 uppercase tracking-tighter">{current.model}</h3>
          <p class="text-slate-600 text-lg leading-relaxed mb-8 max-w-md italic">{current.description}</p>
          <div class="text-4xl font-light text-slate-900 mb-10 border-b border-slate-300 pb-6">
            {current.price.toLocaleString()} €
          </div>
          <a href={`/vehicule/${current.id}`} class="inline-block bg-slate-900 text-white px-10 py-4 text-xs uppercase tracking-widest hover:bg-indigo-600 transition-all">
            Open Dossier
          </a>
        </div>
        {/key}

        <div class="grid grid-cols-2 grid-rows-2 gap-3 h-[550px]">
          {#each [0, 1, 2, 3] as i}
            <div class={`overflow-hidden bg-slate-200 ${i === 0 ? 'row-span-2' : ''}`}>
              {#if current.images[i]}
                <img 
                  src={current.images[i]} 
                  alt={current.model}
                  class="w-full h-full object-cover grayscale hover:grayscale-0 transition-all duration-1000"
                />
              {:else}
                <div class="w-full h-full flex items-center justify-center bg-slate-100 text-[10px] uppercase text-slate-400">No Archive</div>
              {/if}
            </div>
          {/each}
        </div>

      </div>
    {/if}
  </div>
</section>

<style>
  /* On peut ajouter du CSS spécifique ici si besoin */
</style>