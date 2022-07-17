<script>
import { onMount } from "svelte";

  import CuriosityWriter from "./CuriosityWriter.svelte";

  let doc = ""

  function handleType(e) {
    console.log(e.detail)
    localStorage.setItem("doc", e.detail)
    console.log("LS:  " + localStorage.getItem("doc"))
  }

  onMount(() => {
    const docLS = localStorage.getItem("doc")
    if(docLS) {
      doc = docLS
    }
  })
</script>

<CuriosityWriter html={doc} on:type={handleType} />
