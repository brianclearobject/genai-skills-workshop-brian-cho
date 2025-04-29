<script lang="ts">
const url = "https://chat-gemini-732525375974.us-central1.run.app/"
let promptInput: string = ""
let history:{text:string, type:string}[] = []
const chatSend = () => {
    fetch(url, {
        method: "POST",
        headers: {
            "Content-Type": "application/json"
        },
        body: JSON.stringify({user_input: promptInput, history: history})
    }).then(async(res: any) => {
        const data = await res.json()
        history = data["history"]
        promptInput=""
    })

}
</script>
<h1>Chatbot</h1>
<input bind:value={promptInput}/><button onclick={()=>chatSend()}>Send</button>
<div>{#each history as content}
    <p>
        <span>{content["type"]=="user" ? "You" : "Bot"}:</span>
        <span>{content["text"]}</span>    
    </p>
{/each}</div>


