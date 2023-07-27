<script lang="ts">
    import WalletConnect from "$lib/WalletConnect.svelte";
    import MetamaskSign from "$lib/MetamaskSign.svelte";

    export let web3Props: Web3Props;    
    export let jwtToken: string;
</script>

{#if !web3Props?.account}
    <WalletConnect bind:web3Props />
{:else}
    <MetamaskSign bind:web3Props />
{/if}