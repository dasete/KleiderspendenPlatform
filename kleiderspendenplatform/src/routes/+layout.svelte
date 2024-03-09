<script>
  import "../app.css";
  import Footer from "../components/Footer.svelte";
  import Header from "../components/Header.svelte";
</script>
  

<div class="flex flex-col justify-start min-h-screen mx-8">
  <header class="mt-8"> 
    <Header/>
  </header>

  <hr class="w-full h-0.5 mx-1 my-1 bg-slate-300 rounded">

  <main class="flex-1 px-5 py-2">
    <slot />
  </main>

  <hr class="w-full h-0.5 mx-1 my-1 bg-slate-300 rounded">

  <footer class="mb-8">
    <Footer/>
  </footer>
</div>