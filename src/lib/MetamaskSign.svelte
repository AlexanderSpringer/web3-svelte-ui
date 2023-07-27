<script lang="ts">
    import { Signature, ethers } from "ethers";
    import { fly } from "svelte/transition";

    export let web3Props: Web3Props;

    let username: string = "";
    let jwtToken: string = "";

    $: signedIn = false;
    $: loading = false;
    $: updateButtonText = loading ? "Loading..." : "Update";
    $: isValid = username?.length > 2 && username.length < 16
    $: isTouched = username.length > 0;

    async function getNonce() {
        const nonce = await fetch(import.meta.env.VITE_NEST_SERVER + "/auth/nonce", {
            method: "POST",
            headers: {
                "Content-Type": "application/json",
            },
            body: JSON.stringify({ eth_addr: web3Props.account }),
        }).then((res) => res.json());
        return nonce;
    }

    async function signInWithMetamask(message: string) {
        const signature = await web3Props.signer.signMessage(message);
        const jwtToken = await fetch(import.meta.env.VITE_NEST_SERVER + "/auth/metamask", {
            method: "POST",
            headers: {
                "Content-Type": "application/json",
            },
            body: JSON.stringify({ 
                eth_addr: web3Props.account,
                signature: signature,
            }),
        }).then((res) => res.json());
        return jwtToken.access_token;
    }

    async function updateUsername() {
        loading = true;
        await fetch(import.meta.env.VITE_NEST_SERVER + "/users/username", {
            method: "POST",
            headers: {
                "Content-Type": "application/json",
                "Authorization": "Bearer " + jwtToken,
            },
            body: JSON.stringify({ 
                username: username,
            }),
        }).then((res) => res.json());
        loading = false;
    }

    async function getUserProfile() {
        return await fetch(import.meta.env.VITE_NEST_SERVER + "/users/profile", {
            method: "GET",
            headers: {
                "Content-Type": "application/json",
                "Authorization": "Bearer " + jwtToken,
            },
        }).then((res) => res.json());
    }

    async function authenticate() {
        const nonce = await getNonce();
        console.log(nonce);
        jwtToken = await signInWithMetamask(nonce.nonce.toString());
        console.log("jwtToken: ", jwtToken);
        if (jwtToken) {
            signedIn = true;
        }
    }

    async function run() {
        await authenticate();
        const profile = await getUserProfile();
        console.log(profile);
        username = profile.username;
    }

    run();
</script>
    {#if signedIn}
        <div
            class="card bg-base-300 shadow-xl max-w-xl max-h-96"
            in:fly={{ duration: 200, x: 100 }}
            out:fly={{ duration: 100, x: -100 }}
        >
            <figure><img src="https://via.placeholder.com/100x100" alt="Placeholer" /></figure>
            <div class="card-body">
                <h2 class="card-title">Your Public Address:</h2>
                <p>{web3Props.account}</p>
                <h2 class="card-title">Username:</h2>
                <input
                    type="text"
                    placeholder="Username"
                    class="input w-full"
                    bind:value={username}
                    class:input-error={(!isValid && isTouched)}
                    class:input-success={isValid && !loading}
                />
                <div class="card-actions justify-end">
                    <button
                        class="btn btn-primary"
                        on:click={updateUsername}
                        disabled={loading}>
                            {#if loading}
                                <div class="inline-block h-6 w-6 animate-spin rounded-full border-4 border-solid border-current border-r-transparent align-[-0.125em] motion-reduce:animate-[spin_1.5s_linear_infinite]" role="status">
                                    <span class="!absolute !-m-px !h-px !w-px !overflow-hidden !whitespace-nowrap !border-0 !p-0 ![clip:rect(0,0,0,0)]">Loading...</span>
                                </div>
                            {:else}
                                {updateButtonText}
                            {/if}
                        </button
                    >
                </div>
            </div>
        </div>
    {/if}
    {#if !signedIn}
        <!-- Loading Spinner -->
        <div class="inline-block h-8 w-8 animate-spin rounded-full border-4 border-solid border-current border-r-transparent align-[-0.125em] motion-reduce:animate-[spin_1.5s_linear_infinite]" role="status">
            <span class="!absolute !-m-px !h-px !w-px !overflow-hidden !whitespace-nowrap !border-0 !p-0 ![clip:rect(0,0,0,0)]">Loading...</span>
        </div>
    {/if}