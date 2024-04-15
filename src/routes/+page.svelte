<script lang="ts">
    import Game from "$lib/components/Game.svelte";
    import { CellState } from "$lib/types";


    let turn: CellState;
    let winner: CellState | undefined;


    let restart: {() : void};
</script>


<h1 class="text-5xl text-white text-center">Connect 4</h1>
<p class="text-white text-center">
    {#if winner}
        <!-- svelte-ignore a11y-missing-attribute -->
        <a class="{winner == CellState.Red ? "text-red-500" : "text-yellow-400"}">{winner == CellState.Red ? "Red" : "Yellow"}</a> wins!
    {:else}
        <!-- svelte-ignore a11y-missing-attribute -->
        <a class="{turn == CellState.Red ? "text-red-500" : "text-yellow-400"}">{turn == CellState.Red ? "Red" : "Yellow"}</a>'s turn.
    {/if}
</p>

<Game bind:turn={turn} bind:winner={winner} bind:restart={restart} />

{#if winner}
    <button class="border-2 p-2 bg-gray-500 border-gray-600 mx-auto block text-white" on:click={restart}>Restart</button>
{/if}
