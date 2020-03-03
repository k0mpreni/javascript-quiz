<script context="module">
  export async function preload({ params, query }) {
    // the `slug` parameter is available because
    // this file is called [slug].svelte
    const res = await this.fetch(`quiz/${params.slug}.json`);
    const data = await res.json();

    if (res.status === 200) {
      return { question: data };
    } else {
      this.error(res.status, data.message);
    }
  }
</script>

<script>
  export let question;
</script>

<style>
  .content :global(h2) {
    font-size: 1.4em;
    font-weight: 500;
  }

  .content :global(pre) {
    background-color: #f9f9f9;
    box-shadow: inset 1px 1px 5px rgba(0, 0, 0, 0.05);
    padding: 0.5em;
    border-radius: 2px;
    overflow-x: auto;
  }

  .content :global(pre) :global(code) {
    background-color: transparent;
    padding: 0;
  }

  .content :global(ul) {
    line-height: 1.5;
  }

  .content :global(li) {
    margin: 0 0 0.5em 0;
  }

  #choices {
    border: none;
    display: flex;
    flex-direction: column;
  }
</style>

<svelte:head>
  <title>Javascript Question</title>
</svelte:head>

<div class="content">
  <h1>{question.question}</h1>
  {#if question.snippet}
    <pre>
      <code>
        <code>{question.snippet}</code>
      </code>
    </pre>
  {/if}
  <form>
    <fieldset id="choices">
      {#each question.choices as choice}
      <div>
          <input type="radio" id={choice.key} value={choice.value} name="choices" >
          <label for={choice.key}>{choice.key}:  {choice.value}</label>
      </div>
      {/each}
    </fieldset>
  </form>
</div>