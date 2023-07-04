<script lang="ts">
  let textValue: string;
  let lines: number;
  $: lines = textValue ? textValue.split("\n").length: 1


  function addIndentation(event) {
    if (event.key === "Tab") {
      event.preventDefault()
      const { target } = event
        const { selectionStart, selectionEnd } = target
        const indent = '    '
        const value = textValue;
        textValue = value.substring(0, selectionStart) + indent + value.substring(selectionEnd);
        target.selectionStart = target.selectionEnd = selectionStart + indent.length;
    }
  }



  async function postPaste() {
    const response = await fetch("http://paste.itssoap.ninja", {
      method: "POST",
      mode: "cors",

      headers: {
        "Content-Type": "text/html; charset=utf-8",
      }
    })

    const link = response.text()
  }

</script>
<header>
  <div class="header">
    <div class="left">
      <h1><span>SOAP</span> PASTA</h1>
      <img src="/clipboard.svg" alt="" />
    </div>
    <div class="right">
      <!-- svelte-ignore a11y-missing-attribute -->
      <!-- svelte-ignore a11y-click-events-have-key-events -->
      <a on:click={() => textValue= ""} class="delete"><img src="/trash.svg" alt="" /></a>
      <button>Save</button>
    </div>
  </div>
</header>
<main>
  <div class="container">
    <ul class="side-bar">
      {#each { length: lines } as _, index}
        <li>{index + 1}</li>
      {/each}
    </ul>
    <textarea spellcheck="false" on:keypress={addIndentation} placeholder="Paste Here..." bind:value={textValue} class="editor"></textarea>
  </div>
</main>

<style>
  header {
    display: block;
    position: fixed;
    left: 0;
    width: 100%;
    top: 0;
  }

  a {
    display: grid;
    place-items: center;
  }
  .header {
    display: flex;
    background-color: rgb(0, 0, 0);
    text-align: center;
    justify-content: space-between;
    align-items: center;
    padding-block: 0.5rem;
    padding-inline: 1rem;
    /* padding-right: 2rem; */
  }

  .left {
    display: flex;
    gap: 1rem;
    align-items: center;
  }

  .right {
    display: flex;
    gap: 1rem;
  }

  h1 {
    font-size: 1.5rem;
    padding-right: 0.5rem;
    border-right: 1px white solid;
    text-transform: uppercase;
    font-weight: 700;

  }

  h1 span{
    color: rgb(102, 226, 195)
   }

  ul {
    list-style: none;
    text-align: right;
    background-color: #1d1d1d;
    /* font-size: 1.3rem; */
    height: 100%;
    font-size: 1.2rem;
    padding-block-start: 1rem;
    font-family: 'JetBrains Mono', monospace;
    padding-inline-start: 2rem;
    padding-inline-end: 0.7rem;
    border-right: 1px solid rgb(104, 130, 131);
  }

  li {
    min-width: 2rem;
    /* height: 2rem; */
    line-height: 1.4;
    /* padding-block: 0.5rem; */
  }

  .editor {
    padding-block-start: 1rem;
    height: 100%;
    font-size: 1.2rem;
    /* line-height: 2; */
    line-height: 1.4;
    background-color: rgb(31, 36, 44);
    border: none;
    outline: none;
    white-space: nowrap;
    font-family: 'JetBrains Mono', monospace;
    resize: none;
    font-weight: 500;
  }

  .container {
    display: grid;
    grid-template-columns: auto 1fr;
    gap: 0.5rem;
    height: 100%;

  }

  .delete {
    cursor: pointer;
  }

  main {
    height: 93vh;
    padding-top: 4rem;
}


  @media (max-width: 700px) {
    h1 {
      font-size: 1rem;
    }

    ul {
      padding-inline-start: 0.8rem;
    }
  }

</style>
