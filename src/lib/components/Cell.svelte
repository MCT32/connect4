<script lang="ts">
    import { CellState } from "$lib/types";
    
    
    let state: CellState = CellState.Empty;
    let crossed: boolean = false;


    export function set(newState: CellState) {
        if (!newState) throw new Error("Cannot set Empty, use clear()");

        state = newState;
    }

    export function clear() {
        state = CellState.Empty;
        crossed = false;
    }

    export function get() : CellState {
        return state;
    }

    export function cross() {
        crossed = true;
    }
</script>


<div class="w-20 h-20 border-2 border-gray-500 relative">
    {#if state}
        <div class="absolute w-16 h-16 rounded-full {state == CellState.Red ? 'bg-red-600' : 'bg-yellow-400'} m-1" />
    {/if}

    {#if crossed}
        <!-- svelte-ignore a11y-missing-attribute -->
        <img src="/cross.svg" class="absolute z-10">
    {/if}
</div>
