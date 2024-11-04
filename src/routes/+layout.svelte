<script>
  import "../app.css";
</script>
<div class="w-full flex items-center justify-center p-5 ">
  <div class="bg-white w-auto  flex items-center justify-center flex-col rounded-lg p-5">
    <p class="text-xl"><a href="./">svelte-vis-timeline</a></p>
    <div class="flex flex-wrap items-center justify-center gap-3">
      <a href="./drag-and-drop">1. Timeline Drag & Drop </a>
      <a href="./our_drag-and-drop">2. Our timeline</a>
      <a href="./nested-group-3level">3. Nested Group 3level</a>
      <a href="./update-data-on-event">4. update data on event</a>
    </div>
</div>
</div>
<slot />

<style>
  a {
    @apply p-3 underline text-slate-800 hover:text-slate-700  italic font-semibold;
  }
</style>
