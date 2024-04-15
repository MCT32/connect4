<script lang="ts">
    import { CellState } from "$lib/types";
    import Column from "./Column.svelte";


    export let turn: CellState = CellState.Red;
    export let winner: CellState | undefined = undefined;

    let columns: Column[] = new Array(7);

    let onDrop = () => {
        if (turn == CellState.Red) {
            turn = CellState.Yellow;
        } else {
            turn = CellState.Red;
        }

        winner = checkWinner();
    };

    function getColumnStates() {
        let states: CellState[][] = new Array();

        for (let i in columns) {
            states.push(columns[i].getCellStates());
        }

        return states;
    }

    function cross(col: number, row: number) {
        columns[col].cross(row);
    }

    function checkWinner() : CellState | undefined {
        const states = getColumnStates();

        // Check vertical wins
        for (let col = 0; col < 7; col++) {
            rows: for (let row = 0; row < 3; row++) {
                const state = states[col][row];

                if (!state) continue;

                for (let i = 1; i < 4; i++) {
                    if (states[col][row + i] != state) break rows;
                }

                for (let i = 0; i < 4; i++) {
                    cross(col, row + i);
                }

                return state;
            }
        }

        // Check horizontal wins
        for (let row = 0; row < 6; row++) {
            cols: for (let col = 0; col < 4; col++) {
                const state = states[col][row];

                if (!state) continue;

                for (let i = 1; i < 4; i++) {
                    if (states[col + i][row] != state) continue cols;
                }

                for (let i = 0; i < 4; i++) {
                    cross(col + i, row);
                }

                return state;
            }
        }

        // Diagonal wins (\)
        for (let col = 0; col < 4; col++) {
            rows: for (let row = 0; row < 3; row++) {
                const state = states[col][row];

                if (!state) continue;

                for (let i = 1; i < 4; i++) {
                    if (states[col + i][row + i] != state) break rows;
                }

                for (let i = 0; i < 4; i++) {
                    cross(col + i, row + i);
                }

                return state;
            }
        }

        // Diagonal wins (/)
        for (let col = 3; col < 7; col++) {
            rows: for (let row = 0; row < 3; row++) {
                const state = states[col][row];

                if (!state) continue;

                for (let i = 1; i < 4; i++) {
                    if (states[col - i][row + i] != state) break rows;
                }

                for (let i = 0; i < 4; i++) {
                    cross(col - i, row + i);
                }

                return state;
            }
        }
    }

    export function restart() {
        turn = CellState.Red;
        winner = undefined;

        for (let i in columns) {
            columns[i].clear();
        }
    }
</script>


<div class="grid grid-flow-col w-fit m-5">
    {#each {length: 7} as _, i}
        <Column bind:this={columns[i]} bind:turn={turn} bind:winner={winner} bind:onDrop={onDrop} />
    {/each}
</div>
