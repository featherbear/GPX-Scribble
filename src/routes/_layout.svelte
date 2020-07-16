<script>
  import Header from "../components/Header.svelte";
  import Footer from "../components/Footer.svelte";

  export let segment;
</script>

<style lang="scss">
  .container {
    display: flex;
    min-height: 100vh;
    flex-direction: column;

    main {
      flex: 1;
    }

    .footer {
      flex-shrink: 0;
    }
  }
</style>

<div class="container">
  <Header {segment} />
  <slot />
  <footer class="footer">
    <Footer />
  </footer>
</div>
