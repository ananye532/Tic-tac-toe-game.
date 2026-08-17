Code available for the game tic-tac toe, it is a workable and executable code for the game-
the entire code is written in HTML/CSS.



<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Tic Tac Toe</title>
<style>
  body {
    font-family: system-ui, sans-serif;
    background: #1e1e2e;
    color: #fff;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    min-height: 100vh;
    margin: 0;
  }
  h1 { margin-bottom: 0.5rem; }
  #status { margin-bottom: 1rem; font-size: 1.1rem; min-height: 1.5rem; }
  #board {
    display: grid;
    grid-template-columns: repeat(3, 100px);
    grid-template-rows: repeat(3, 100px);
    gap: 6px;
  }
  .cell {
    background: #2a2a3d;
    border: none;
    border-radius: 8px;
    font-size: 2.5rem;
    font-weight: bold;
    color: #fff;
    cursor: pointer;
    display: flex;
    align-items: center;
    justify-content: center;
    transition: background 0.15s;
  }
  .cell:hover:not(:disabled) { background: #3a3a55; }
  .cell:disabled { cursor: default; }
  .x { color: #61dafb; }
  .o { color: #f78c6c; }
  #reset {
    margin-top: 1.2rem;
    padding: 0.6rem 1.4rem;
    background: #61dafb;
    border: none;
    border-radius: 6px;
    font-size: 1rem;
    font-weight: bold;
    color: #1e1e2e;
    cursor: pointer;
  }
  #reset:hover { background: #4fc3ec; }
</style>
</head>
<body>

<h1>Tic Tac Toe</h1>
<div id="status">Player X's turn</div>
<div id="board"></div>
<button id="reset">Restart</button>

<script>
  const boardEl = document.getElementById('board');
  const statusEl = document.getElementById('status');
  const resetBtn = document.getElementById('reset');

  let board = Array(9).fill(null);
  let currentPlayer = 'X';
  let gameOver = false;

  const winPatterns = [
    [0,1,2], [3,4,5], [6,7,8], // rows
    [0,3,6], [1,4,7], [2,5,8], // columns
    [0,4,8], [2,4,6]           // diagonals
  ];

  function checkWinner() {
    for (const pattern of winPatterns) {
      const [a, b, c] = pattern;
      if (board[a] && board[a] === board[b] && board[a] === board[c]) {
        return { winner: board[a], pattern };
      }
    }
    if (board.every(cell => cell !== null)) {
      return { winner: 'draw' };
    }
    return null;
  }

  function render() {
    boardEl.innerHTML = '';
    board.forEach((value, i) => {
      const btn = document.createElement('button');
      btn.className = 'cell' + (value === 'X' ? ' x' : value === 'O' ? ' o' : '');
      btn.textContent = value || '';
      btn.disabled = value !== null || gameOver;
      btn.addEventListener('click', () => handleMove(i));
      boardEl.appendChild(btn);
    });
  }

  function handleMove(i) {
    if (board[i] || gameOver) return;
    board[i] = currentPlayer;

    const result = checkWinner();
    if (result) {
      gameOver = true;
      if (result.winner === 'draw') {
        statusEl.textContent = "It's a draw!";
      } else {
        statusEl.textContent = `Player ${result.winner} wins!`;
      }
    } else {
      currentPlayer = currentPlayer === 'X' ? 'O' : 'X';
      statusEl.textContent = `Player ${currentPlayer}'s turn`;
    }
    render();
  }

  function resetGame() {
    board = Array(9).fill(null);
    currentPlayer = 'X';
    gameOver = false;
    statusEl.textContent = "Player X's turn";
    render();
  }

  resetBtn.addEventListener('click', resetGame);
  render();
</script>

</body>
</html>
