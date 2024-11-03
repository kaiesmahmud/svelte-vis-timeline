<script>
    import "../app.css";
  </script>
  <div class="flex items-center justify-center flex-col w-full">
    <p>Pages</p>
    <div class="flex flex-wrap items-center justify-center">
      <a href="./drag-and-drop">1. Timeline Drag & Drop Example</a>
    </div>
  </div>
  <slot />
  <style>
    a{
      @apply p-5 underline text-slate-800 hover:text-slate-700  italic font-semibold ;
    }
  </style>