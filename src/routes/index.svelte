<script lang="ts">
    let token = "";
    let promise = null;
    function send() {
        if (token?.toLowerCase().startsWith("bearer")) {
            token = token.substring("bearer".length).trim()
        }
        token = token.trim();
        promise = fetch("https://login.dnbregnskap.dnb.no/External/Disconnect?scheme=bankid", {method: "POST", headers: {Authorization: `Bearer ${token}`}});
    }

    function formatToken(e) {
        const data = e.target.value;

        if (data?.toLowerCase().startsWith("bearer")) {
            token = data.substring("bearer".length).trim()
        }

    }

</script>

<h1>Fyll inn token og trykk send</h1>


<textarea bind:value={token} on:input={formatToken} placeholder="Token her"></textarea>
<br>
<button on:click={send}>Send</button>

{#if promise}
    <br>
    {#await promise then res}
        {#if res.status === 404}
            Bruker finnes ikke eller er allerede koblet av
        {:else}
            Success...
        {/if}
    {:catch err}
        {err}
    {/await}
{/if}