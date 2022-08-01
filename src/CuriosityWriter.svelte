<script>
  import {createEventDispatcher, onMount, } from "svelte";
  import { get_current_component } from "svelte/internal"
  const dispatch = createEventDispatcher();
  const component = get_current_component()

  const emptyText = `<h1></br></h1>`
  export let html = emptyText
  $: if(html.length === 0) html = emptyText

  $: dispatchEvent = (name, detail) => {
    dispatch(name, detail)
    component.dispatchEvent && component.dispatchEvent(new CustomEvent("type", { detail }))
  }

  $: dispatchEvent("type", html)

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

  function actionBold() {
    document.execCommand("bold", false);
  } 
  function actionItalic() {
    document.execCommand("italic", false);
  }
  function actionHeader() {
    document.execCommand("formatBlock", false, "h3");
  }
  function actionText() {
    document.execCommand("formatBlock", false, "p");
  }
  function actionLink() {
    const url = prompt("Insert your link");
    document.execCommand("createLink", true, url);
  }

  $: isPlaceholderShouldShow = html === emptyText || html === "<h1><br></h1>"

</script>

<section>
  <main class="curiosity" use:focus use:writingAction bind:innerHTML={html} class:placeholder={isPlaceholderShouldShow} contenteditable="true">
    {html}
  </main>
</section>

<div class="editor">
  <div on:mousedown|preventDefault on:click|preventDefault={actionBold} class="action bold">
    <svg width="24" height="24" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
      <path d="M12 12.75H4.88C4.47 12.75 4.13 12.41 4.13 12V4.5C4.13 2.98 5.36 1.75 6.88 1.75H12C15.03 1.75 17.5 4.22 17.5 7.25C17.5 10.28 15.03 12.75 12 12.75ZM5.62 11.25H12C14.21 11.25 16 9.46 16 7.25C16 5.04 14.21 3.25 12 3.25H6.88C6.19 3.25 5.63 3.81 5.63 4.5V11.25H5.62Z" fill="white"/>
      <path d="M14.38 22.25H6.88C5.36 22.25 4.13 21.02 4.13 19.5V12C4.13 11.59 4.47 11.25 4.88 11.25H14.38C17.41 11.25 19.88 13.72 19.88 16.75C19.88 19.78 17.41 22.25 14.38 22.25ZM5.62 12.75V19.5C5.62 20.19 6.18 20.75 6.87 20.75H14.37C16.58 20.75 18.37 18.96 18.37 16.75C18.37 14.54 16.58 12.75 14.37 12.75H5.62Z" fill="white"/>
      </svg>
  </div>
  <div on:mousedown|preventDefault on:click|preventDefault={actionItalic} class="action italic">
    <svg width="24" height="24" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
      <path d="M18.88 3.75H9.62C9.21 3.75 8.88 3.41 8.88 3C8.88 2.59 9.22 2.25 9.63 2.25H18.88C19.29 2.25 19.63 2.59 19.63 3C19.63 3.41 19.29 3.75 18.88 3.75Z" fill="white"/>
      <path d="M14.38 21.75H5.12C4.71 21.75 4.37 21.41 4.37 21C4.37 20.59 4.71 20.25 5.12 20.25H14.37C14.78 20.25 15.12 20.59 15.12 21C15.12 21.41 14.79 21.75 14.38 21.75Z" fill="white"/>
      <path d="M9.75001 21.75C9.69001 21.75 9.63001 21.74 9.57001 21.73C9.17001 21.63 8.92001 21.22 9.02001 20.82L13.52 2.82C13.62 2.42 14.02 2.17 14.43 2.27C14.83 2.37 15.08 2.78 14.98 3.18L10.48 21.18C10.39 21.52 10.09 21.75 9.75001 21.75Z" fill="white"/>
      </svg>
        </div>
  <div class="action free"></div>
  <div on:mousedown|preventDefault on:click|preventDefault={actionHeader} class="action header">
    <svg width="24" height="24" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
      <path d="M21.33 7.92C20.92 7.92 20.58 7.58 20.58 7.17V5.35C20.58 4.62 19.99 4.03 19.26 4.03H4.74C4.01 4.03 3.42 4.62 3.42 5.35V7.18C3.42 7.59 3.08 7.93 2.67 7.93C2.26 7.93 1.92 7.59 1.92 7.17V5.35C1.92 3.79 3.19 2.53 4.74 2.53H19.26C20.82 2.53 22.08 3.8 22.08 5.35V7.18C22.08 7.59 21.75 7.92 21.33 7.92Z" fill="white"/>
      <path d="M12 21.47C11.59 21.47 11.25 21.13 11.25 20.72V4.11C11.25 3.7 11.59 3.36 12 3.36C12.41 3.36 12.75 3.7 12.75 4.11V20.72C12.75 21.14 12.41 21.47 12 21.47Z" fill="white"/>
      <path d="M15.94 21.47H8.06C7.65 21.47 7.31 21.13 7.31 20.72C7.31 20.31 7.65 19.97 8.06 19.97H15.94C16.35 19.97 16.69 20.31 16.69 20.72C16.69 21.13 16.35 21.47 15.94 21.47Z" fill="white"/>
      </svg>
        </div>
  <div on:mousedown|preventDefault on:click|preventDefault={actionText} class="action text">
    <svg width="24" height="24" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
      <path d="M18.61 6.68C18.2 6.68 17.86 6.34 17.86 5.93V4.42C17.86 3.82 17.37 3.32 16.76 3.32H3.82999C3.22999 3.32 2.72999 3.81 2.72999 4.42V5.93C2.72999 6.34 2.38999 6.68 1.97999 6.68C1.56999 6.68 1.23999 6.34 1.23999 5.93V4.42C1.23999 2.99 2.40999 1.82 3.83999 1.82H16.76C18.19 1.82 19.36 2.98 19.36 4.42V5.93C19.36 6.34 19.02 6.68 18.61 6.68Z" fill="white"/>
      <path d="M10.3 18.85C9.88999 18.85 9.54999 18.51 9.54999 18.1V3.32C9.54999 2.91 9.88999 2.57 10.3 2.57C10.71 2.57 11.05 2.91 11.05 3.32V18.1C11.05 18.52 10.71 18.85 10.3 18.85Z" fill="white"/>
      <path d="M12.48 18.85H6.89999C6.48999 18.85 6.14999 18.51 6.14999 18.1C6.14999 17.69 6.48999 17.35 6.89999 17.35H12.48C12.89 17.35 13.23 17.69 13.23 18.1C13.23 18.51 12.89 18.85 12.48 18.85Z" fill="white"/>
      <path d="M22.01 13.2C21.6 13.2 21.26 12.86 21.26 12.45V11.65C21.26 11.34 21 11.08 20.69 11.08H13.68C13.27 11.08 12.93 10.74 12.93 10.33C12.93 9.92 13.27 9.58 13.68 9.58H20.69C21.83 9.58 22.76 10.51 22.76 11.65V12.45C22.76 12.87 22.43 13.2 22.01 13.2Z" fill="white"/>
      <path d="M16.08 22.18C15.67 22.18 15.33 21.84 15.33 21.43V10.87C15.33 10.46 15.67 10.12 16.08 10.12C16.49 10.12 16.83 10.46 16.83 10.87V21.43C16.83 21.84 16.49 22.18 16.08 22.18Z" fill="white"/>
      <path d="M18.22 22.18H13.94C13.53 22.18 13.19 21.84 13.19 21.43C13.19 21.02 13.53 20.68 13.94 20.68H18.22C18.63 20.68 18.97 21.02 18.97 21.43C18.97 21.84 18.63 22.18 18.22 22.18Z" fill="white"/>
      </svg>
      
  </div>
  <div on:mousedown|preventDefault on:click|preventDefault={actionLink} class="action link">
    <svg width="24" height="24" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
      <path d="M12.5002 14.75H10.0002C9.59024 14.75 9.25024 14.41 9.25024 14C9.25024 13.59 9.59024 13.25 10.0002 13.25H12.5002C15.1202 13.25 17.2502 11.12 17.2502 8.5C17.2502 5.88 15.1202 3.75 12.5002 3.75H7.50024C4.88024 3.75 2.75024 5.88 2.75024 8.5C2.75024 9.6 3.14023 10.67 3.84023 11.52C4.10023 11.84 4.06023 12.31 3.74023 12.58C3.42023 12.84 2.95024 12.8 2.68024 12.48C1.75024 11.36 1.24023 9.95 1.24023 8.5C1.24023 5.05 4.04023 2.25 7.49023 2.25H12.4902C15.9402 2.25 18.7402 5.05 18.7402 8.5C18.7402 11.95 15.9502 14.75 12.5002 14.75Z" fill="white"/>
      <path d="M16.5 21.75H11.5C8.05 21.75 5.25 18.95 5.25 15.5C5.25 12.05 8.05 9.25 11.5 9.25H14C14.41 9.25 14.75 9.59 14.75 10C14.75 10.41 14.41 10.75 14 10.75H11.5C8.88 10.75 6.75 12.88 6.75 15.5C6.75 18.12 8.88 20.25 11.5 20.25H16.5C19.12 20.25 21.25 18.12 21.25 15.5C21.25 14.4 20.86 13.33 20.16 12.48C19.9 12.16 19.94 11.69 20.26 11.42C20.58 11.15 21.05 11.2 21.32 11.52C22.25 12.64 22.76 14.05 22.76 15.5C22.75 18.95 19.95 21.75 16.5 21.75Z" fill="white"/>
      </svg>  
  </div>
</div>

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

  .placeholder::before {
    content: "Write your story";
    font-weight: 700;
    font-size: 30px;
    opacity: 0.3;
    position: absolute;
  }


  /* for editor */
  .editor {
    position: fixed;
    bottom: 0;
    left: 0;
    right: 0;
    background-color: #242526;
    height: 44px;
    display: flex;
    justify-content:space-between ;
    align-items: center;
    padding: 0 20px;
  }

  .action {
    height: 100%;
    flex-grow: 1;
    margin: 5px;
    display: flex;
    justify-content: center;
    align-items: center;
    width: 32px;
  }

  .action:active {
    background-color: #25C19F;
  }

  .action svg {
    height: 18px;
  }
  /* for editor */

  section {
    font-family: "Lexend", sans-serif;
    background-color: #1B1B1D;
    color: #E3E3E3;
  }

  main {
    margin: 0 auto;
    padding-top: 72px;
    padding-bottom: 240px;
    padding-right: 18px;
    padding-left: 18px;
  }

  .curiosity :global(h1) {
    font-weight: 700;
    font-size: 30px;
    margin-bottom: 40px;
    line-height: 1.3;
  }

  .curiosity :global(h2) {
    font-weight: 300;
    color: grey;
    font-size: 22px;
    margin-top: -32px;
    margin-bottom: 32px;
    line-height: 1.4;
  }

  .curiosity :global(h3) {
    font-weight: 700;
    font-size: 20px;
    margin-bottom: 12px;
    margin-top: 48px;
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
    font-weight: 300;
    font-size: 16px;
    line-height: 1.8;
    margin-bottom: 24px;
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

  @media (min-width: 672px) {
  main {
    max-width: 778px;
    padding-right: 24px;
    padding-left: 24px;
  }

  .placeholder::before {
    content: "Write your story";
    font-size: 36px;
  }

  .curiosity :global(h1) {
    font-size: 36px;
  }

  .curiosity :global(h2) {
    font-size: 28px;
  }

  .curiosity :global(h3) {
    font-size: 26px;
  }

  .curiosity :global(h4) {
    font-size: 18px;
  }

  .curiosity :global(p) {
    font-size: 20px;
    line-height: 1.9;
  }

  .editor {
    /* display: none; */
    max-width: 360px;
    margin: 0 auto;
    bottom: 16px;
    border-radius: 18px;
  }

  .editor div {
    cursor: pointer;
  }
}
</style>