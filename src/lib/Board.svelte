<script>
    const Types = {
        NOT: 1,
        SELECTED: 2,
        ERROR: 3
    }

    const Result = {
        ERROR: 0,
        SUCCESS: 1
    }

    const BOARD_SIZE = 8;

    export let result = null;
    let counter = 0;
    let board = [];

    for (let x = 0; x < BOARD_SIZE; x++) {
        board.push([]);
        for (let y = 0; y < BOARD_SIZE; y++) {
            board[x].push(Types.NOT);
        }
    }

    const check = (x, y) => (e) => {
        if ([Types.SELECTED, Types.ERROR].includes(board[x][y])) {
            counter -= 1;
            unMarker(x, y, e);
        } else if (counter < 8) {
            counter += 1;
            marker(x, y, e);
        }

        if (counter === BOARD_SIZE) {
            if (boardCheck()) {
                result = Result.ERROR;
            } else {
                result = Result.SUCCESS;
            }
        } else {
            result = null;
        }
    }

    const marker = (x, y, e) => {
        e.target.classList.add('active');
        board[x][y] = Types.SELECTED;
    }

    const unMarker = (x, y, e) => {
        e.target.classList.remove('active');
        board[x][y] = Types.NOT;
    }

    const boardCheck = () => {
        let check = false;

        for (let x = 0; x < BOARD_SIZE; x++) {
            for (let y = 0; y < BOARD_SIZE; y++) {
                if (board[x][y] === Types.SELECTED) {
                    for (let z = 0; z < 8; z++)
                        if (board[x][z] !== Types.NOT && z !== y) {
                            check = true;
                            board[x][z] = Types.ERROR
                        }
                    for (let z = 0; z < 8; z++)
                        if (board[z][y] !== Types.NOT && z !== x) {
                            check = true;
                            board[z][y] = Types.ERROR
                        }

                    let a = x, b = y;
                    while (a < 8 && b < 8) {
                        if (board[a][b] !== Types.NOT && a !== x) {
                            check = true;
                            board[a][b] = Types.ERROR;
                        }
                        a++;
                        b++;
                    }

                    a = x;
                    b = y;
                    while (a >= 0 && b >= 0) {
                        if (board[a][b] !== Types.NOT && a !== x) {
                            check = true;
                            board[a][b] = Types.ERROR;
                        }
                        a--;
                        b--;
                    }

                    a = x;
                    b = y;
                    while (a < 8 && b >= 0) {
                        if (board[a][b] !== Types.NOT && a !== x) {
                            check = true;
                            board[a][b] = Types.ERROR;
                        }
                        a++;
                        b--;
                    }

                    a = x;
                    b = y;
                    while (a >= 0 && b < 8) {
                        if (board[a][b] !== Types.NOT && a !== x) {
                            check = true;
                            board[a][b] = Types.ERROR;
                        }
                        a--;
                        b++;
                    }
                }
            }
        }
        return check;
    }

</script>

{#each board as boardX, x}
    {#each boardX as boardY, y}
        <button on:click={check(x, y)}
                class:active={board[x][y] === Types.SELECTED}
                class:error={board[x][y] === Types.ERROR}
        ></button>
    {/each}
{/each}
