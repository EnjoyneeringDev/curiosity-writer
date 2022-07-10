<script>
  import {createEventDispatcher, onMount, } from "svelte";
  const dispatch = createEventDispatcher();

  export let theme = "dark"

  const emptyText = `<h1></br></h1>`
  let html = emptyText
  $: if(html.length === 0) html = emptyText

  $: dispatch("type", {
    html
  })

  onMount(() => {
    document.execCommand("defaultParagraphSeparator", false, "p");
  })
  
  function writingAction(element) {
    element.addEventListener("keydown", function(e) {
      if (e.ctrlKey && e.key === "p") {
        e.preventDefault();
        document.execCommand("formatBlock", false, "p");
      } else if (e.ctrlKey && e.key === "1") {
        e.preventDefault();
        document.execCommand("formatBlock", false, "h1");
      } else if (e.ctrlKey && e.key === "2") {
        e.preventDefault();
        document.execCommand("formatBlock", false, "h2");
      } else if (e.ctrlKey && e.key === "q") {
        e.preventDefault();
        document.execCommand("formatBlock", false, "h4");
      }  else if (e.ctrlKey && e.key === "h") {
        e.preventDefault();
        document.execCommand("formatBlock", false, "h3");
      } else if (e.ctrlKey && e.key === "l") {
        e.preventDefault();
        const url = prompt("Enter your link");
        document.execCommand("createLink", true, url);
      } 
    })
  }

  function focus(element) {
    element.focus()
  }


</script>

<section>
  <main class:dark = {theme === "dark"} class:light ={theme === "light"} class="curiosity" use:focus use:writingAction bind:innerHTML={html} contenteditable="true">
  </main>
</section>

<style>
  @import url("https://fonts.googleapis.com/css2?family=Lexend:wght@300;400;700&display=swap");

  :global(*)  {
    outline: none;
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }

  :global(body) {
    background-color: #1B1B1D;
  }

  section {
    font-family: "Lexend", sans-serif;
  }

  .dark {
    background-color: #1B1B1D;
    color: #E3E3E3;
  }

  .light {
    background-color: #FFFFFF;
    color: #1C1E21;
  }

  main {
    margin: 0 auto;
    padding-top: 72px;
    padding-bottom: 240px;
    padding-right: 24px;
    padding-left: 24px;
  }

  .curiosity :global(h1) {
    font-weight: 700;
    font-size: 32px;
    margin-bottom: 40px;
    line-height: 1.3;
  }

  .curiosity :global(h2) {
    font-weight: 400;
    color: grey;
    font-size: 22px;
    margin-top: -32px;
    margin-bottom: 32px;
    line-height: 1.4;
  }

  .curiosity :global(h3) {
    font-weight: 700;
    font-size: 22px;
    margin-bottom: 12px;
    margin-top: 38px;
    line-height: 1.2;
  }

  .curiosity :global(h4)  {
    font-weight: 400;
    margin: 0 auto;
    font-size: 16px;
    line-height: 1.8;
    margin-top: 54px;
    margin-bottom: 44px;
    padding-left: 16px;
    padding-right: 16px;
  }

  .curiosity :global(p) {
    font-weight: 400;
    font-size: 18px;
    line-height: 1.8;
    margin-bottom: 20px;
    max-width: 65ch;
  }

  .curiosity :global(a) {
    text-decoration: none;
    color: #25C19F;
  }

  .curiosity :global(a:hover) {
    cursor: pointer;
    text-decoration: underline;
    color: #25C19F;
  }

  .curiosity :global(ul) {
    font-size: 20px;
    padding-left: 20px;
    margin-bottom: 24px;
  }

  .curiosity :global(li) {
    margin-bottom: 8px;
  }

  .curiosity :global(img) {
    cursor: pointer;
    max-width: 100%;
    max-height: 640px;
    object-fit: cover;
    display: block;
    margin-left: auto;
    margin-right: auto;
  }

  @media (min-width: 778px) {
  main {
    max-width: 778px;
  }
}
</style>