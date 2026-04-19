<script lang="ts">
  const calendarId = "AcZssZ1JQ_Wfia_mLSsXdvQ5zmTp6QnCV4XUT5c7Wg4ecU-AC5zwC3TMobaBEN59GlVRQkJwaTSwxhvV"; 

  function openBooking(e: MouseEvent) {
    e.preventDefault();
    
    const global = window as any;
    const bookingUrl = `https://calendar.google.com/calendar/appointments/schedules/${calendarId}?gv=true`;

    console.log("Tentative d'ouverture du calendrier...");

    if (global.calendar && global.calendar.schedulingButton) {
      try {
        global.calendar.schedulingButton.load({
          url: bookingUrl,
          color: '#0f172a', 
          label: 'Book a consultation',
          target: document.body,
        });
      } catch (err) {
        console.error("Erreur Google Script:", err);
        window.open(bookingUrl, '_blank');
      }
    } else {
      console.warn("Script Google non détecté, ouverture dans un nouvel onglet.");
      window.open(bookingUrl, '_blank');
    }
  }
</script>

<svelte:head>
  <link href="https://calendar.google.com/calendar/scheduling-button/scp.css" rel="stylesheet">
  <script src="https://calendar.google.com/calendar/scheduling-button/scp.js" async></script>
</svelte:head>

<button 
  onclick={openBooking}
  type="button"
  class="border border-slate-900 text-slate-900 px-8 py-4 text-[10px] uppercase tracking-[0.3em] hover:cursor-pointer hover:bg-slate-900 hover:text-white transition-all font-bold"
>
  Book a call
</button>