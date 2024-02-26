<script>
  import { onMount } from "svelte";
  import logo from "$lib/assets/Logo.png";
  import menuButton from "$lib/assets/MenuButton.png";

  let showMobileMenu = false;

  onMount(() => {
    // Define your media query
    const mediaQuery = window.matchMedia("(max-width: 768px)");
    
    // Call the function initially to set the boolean based on the initial viewport size
    checkIfMobile();

    // Add event listener for changes in the media query
    mediaQuery.addEventListener("change", checkIfMobile);
  });

  function toggleMobileMenu() {
    showMobileMenu = !showMobileMenu;
  }

  function checkIfMobile() {
    if (showMobileMenu) {
      showMobileMenu = false;
    }
  }

</script>

<!-- Navbar -->
<nav>
  <div class="flex justify-between">
    <!-- Logo & Title -->
    <a href="/" class="flex items-center item py-4 hover:text-emerald-300">
      <img class="h-12 mr-2" src={logo} alt="img">
      <span class="font-bold text-xl px-2">Apparel Relief</span>
    </a>

    <!-- Nav Menu -->
    <div class="hidden md:flex items-center space-x-2">
      <a href="/register" class = "py-4 px-2 hover:text-emerald-300">Kleiderspende</a>
      <a href="/news" class = "py-4 px-2 hover:text-emerald-300">Neuigkeiten</a>
      <a href="/about" class = "py-4 px-2 hover:text-emerald-300">Über Uns</a>
    </div>

    <!-- Mobile Button -->
    <div class="md:hidden flex items-center space-x-2">
      <button class="px-2 py-2 rounded hover:bg-slate-700 transition duration-200 active:bg-slate-500" class:bg-slate-500={showMobileMenu} on:click={toggleMobileMenu}>
        <img class="h-8" src={menuButton} alt="menuButton">
      </button>
    </div>
  </div>


  <!-- Mobile Menu -->
  {#if showMobileMenu}
    <div class="pb-3">
      <hr class="w-full h-0.5 mx-1 my-1 bg-slate-300 rounded">
      <a href="/register" class = "block py-1 hover:text-emerald-300 hover:bg-slate-700 transition duration-100 text-center">Kleiderspende</a>
      <a href="/news" class = "block py-1 hover:text-emerald-300 hover:bg-slate-700 transition duration-100 text-center">Neuigkeiten</a>
      <a href="/about" class = "block py-1 hover:text-emerald-300 hover:bg-slate-700 transition duration-100 text-center">Über Uns</a>
    </div>
  {/if}
</nav>
