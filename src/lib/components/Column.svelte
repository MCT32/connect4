<script lang="ts">
    import Cell from "./Cell.svelte";
    import { CellState } from "$lib/types";


    let cells: Cell[] = new Array(6);

    export let winner: CellState | undefined;
    export let turn: CellState;


    export let onDrop: {(): void} | undefined = undefined;

    export function drop(token: CellState) {
        if (!token) throw new Error("Cannot drop Empty");

        if (cells[0].get()) throw new Error("Column is full");

        let place = 0;

        for (let i = 1; i < cells.length; i++) {
            if (cells[i].get() != CellState.Empty) break;

            place = i;
        }

        cells[place].set(token);

        if (onDrop) onDrop();
    }

    function onClick() {
        if (!winner) drop(turn);
    }

    export function getCellStates() {
        let states: CellState[] = new Array();

        for (let i of cells) {
            states.push(i.get());
        }

        return states;
    }

    export function cross(row: number) {
        cells[row].cross();
    }

    export function clear() {
        for (let i in cells) {
            cells[i].clear();
        }
    }
</script>


<!-- svelte-ignore a11y-no-static-element-interactions -->
<!-- svelte-ignore a11y-click-events-have-key-events -->
<div on:click={onClick}>
    {#each {length: 6} as _, i}
        <Cell bind:this={cells[i]} />
    {/each}
</div>
