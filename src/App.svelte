<script>
import { onMount } from "svelte";

  import CuriosityWriter from "./CuriosityWriter.svelte";

  let doc = ""

  function handleType(e) {
    console.log(e.detail)
    doc = e.detail
  }


  onMount(() => {
    const docLS = localStorage.getItem("doc")
    if(doc) {
      doc = docLS
    }
  })
</script>

<CuriosityWriter html={doc} on:type={handleType} />
