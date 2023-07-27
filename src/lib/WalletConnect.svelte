<script lang="ts">
    import { ethers } from "ethers";
    export let web3Props: Web3Props;

    async function connectWallet() {
        let provider = new ethers.BrowserProvider(window.ethereum, 'any');
        await provider.send('eth_requestAccounts', []);
        const signer = await provider.getSigner();
        const account = await signer.getAddress();
        const chainId = await (await signer.provider.getNetwork()).chainId;
        web3Props = { signer, provider, chainId, account };
        console.log(web3Props);
    }
</script>

{#if !web3Props?.account}
    <button class="btn btn-primary" on:click={connectWallet}>Connect Wallet</button>
{/if}