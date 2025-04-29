<script lang="ts">
  const url = "https://chat-gemini-732525375974.us-central1.run.app/";
  let promptInput: string = "";
  let history: { text: string; type: string }[] = [];
  let errorMsg: string | null = null;
  let loading: boolean = false;
  let userInput: string;

  const chatSend = () => {
    userInput = promptInput;
    loading = true;
    promptInput = "";
    fetch(url, {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify({ user_input: userInput, history: history }),
    })
      .then(async (res: any) => {
        const data = await res.json();
        history = data["history"];
        errorMsg = null;
      })
      .catch(() => {
        errorMsg = "Something went wrong, please refresh to try again.";
      })
      .finally(() => {
        loading = false;
      });
    return;
  };
</script>

<h1>Chatbot</h1>
<input bind:value={promptInput} /><button onclick={() => chatSend()}
  >Send</button
>
<div>
  {#each history as content}
    <p>
      <span>{content["type"] == "user" ? "You" : "Bot"}:</span>
      <span>{content["text"]}</span>
    </p>
  {/each}
</div>
{#if loading}
  <div>
    <p>
      <span>You: {userInput}</span>
    </p>
    <p>
      <span>Loading...</span>
    </p>
  </div>
{/if}
{#if errorMsg}<div><span>{errorMsg}</span></div>{/if}
