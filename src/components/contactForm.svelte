<script lang="ts">
  import { onMount } from 'svelte';
  import { animate, stagger } from 'motion';
  
  let name = $state("");
  let email = $state("");
  let subject = $state("Demande d'information");
  let message = $state("");
  let status = $state(""); // "sending", "success", "error"

  onMount(() => {
    animate(
      ".form-field",
      { opacity: [0, 1], y: [10, 0] },
      { delay: stagger(0.05), duration: 0.6, easing: [0.22, 1, 0.36, 1] }
    );
  });

  async function handleSubmit(e?: Event) {
    if (e) e.preventDefault();
    status = "sending";

    try {
      const res = await fetch(`${import.meta.env.PUBLIC_API_URL}/api/junctionretro/contact`, {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({ name, email, subject, message })
      });
      
      if (!res.ok) throw new Error("Failed to send");
      
      status = "success";
      name = ""; email = ""; message = "";
    } catch (err) {
      console.error(err);
      status = "error";
    }
  }
</script>

<form onsubmit={handleSubmit} class="space-y-6">
  <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
    <div class="space-y-2 form-field opacity-0">
      <label for="name" class="text-[10px] uppercase tracking-widest text-slate-500 font-bold">Nom Complet</label>
      <input 
        id="name"
        type="text" 
        bind:value={name}
        required
        placeholder="Jean Dupont" 
        class="w-full bg-slate-50 border border-slate-200 px-4 py-3 text-sm focus:border-slate-900 focus:outline-none transition-colors" 
      />
    </div>
    <div class="space-y-2 form-field opacity-0">
      <label for="email" class="text-[10px] uppercase tracking-widest text-slate-500 font-bold">Adresse Email</label>
      <input 
        id="email"
        type="email" 
        bind:value={email}
        required
        placeholder="jean@example.com" 
        class="w-full bg-slate-50 border border-slate-200 px-4 py-3 text-sm focus:border-slate-900 focus:outline-none transition-colors" 
      />
    </div>
  </div>

  <div class="space-y-2 form-field opacity-0">
    <label for="subject" class="text-[10px] uppercase tracking-widest text-slate-500 font-bold">Objet</label>
    <div class="relative">
      <select 
        id="subject"
        bind:value={subject}
        class="w-full bg-slate-50 border border-slate-200 px-4 py-3 text-sm focus:border-slate-900 focus:outline-none transition-colors appearance-none"
      >
        <option>Demande d'information</option>
        <option>Vendre un véhicule</option>
        <option>Expertise / Consultation</option>
      </select>
      <div class="absolute right-4 top-1/2 -translate-y-1/2 pointer-events-none text-slate-400">
        <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-4 h-4">
          <path stroke-linecap="round" stroke-linejoin="round" d="m19.5 8.25-7.5 7.5-7.5-7.5" />
        </svg>
      </div>
    </div>
  </div>

  <div class="space-y-2 form-field opacity-0">
    <label for="message" class="text-[10px] uppercase tracking-widest text-slate-500 font-bold">Message</label>
    <textarea 
      id="message"
      bind:value={message}
      required
      rows="5" 
      placeholder="Votre message..." 
      class="w-full bg-slate-50 border border-slate-200 px-4 py-3 text-sm focus:border-slate-900 focus:outline-none transition-colors resize-none"
    ></textarea>
  </div>

  <button 
    type="submit"
    disabled={status === 'sending'}
    class="w-full bg-slate-900 text-white py-4 text-xs uppercase tracking-[0.2em] font-bold hover:bg-black transition-all disabled:opacity-50 disabled:cursor-not-allowed form-field opacity-0"
  >
    {#if status === 'sending'}
      Envoi en cours...
    {:else}
      Envoyer la demande
    {/if}
  </button>

  {#if status === 'success'}
    <div 
      class="p-4 bg-green-50 border border-green-100 text-green-700 text-xs text-center font-medium animate-in fade-in slide-in-from-bottom-2 duration-500"
    >
      Votre message a été envoyé avec succès. Nous vous recontacterons sous peu.
    </div>
  {:else if status === 'error'}
    <div 
      class="p-4 bg-red-50 border border-red-100 text-red-700 text-xs text-center font-medium animate-in fade-in slide-in-from-bottom-2 duration-500"
    >
      Une erreur est survenue. Veuillez réessayer plus tard.
    </div>
  {/if}
</form>