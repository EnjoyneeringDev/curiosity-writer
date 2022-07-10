<script>
  import CuriosityWriter from "./CuriosityWriter.svelte";

  function handleType(e) {
    console.log(e.detail.html)
  }
</script>

<CuriosityWriter on:type={handleType} />
