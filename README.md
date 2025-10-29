<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Puzzle Game</title>
  <style>
    body {
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      min-height: 100vh;
      background: linear-gradient(135deg, #4f46e5, #06b6d4);
      font-family: 'Poppins', sans-serif;
      color: white;
    }

    h1 {
      margin-bottom: 20px;
      font-size: 2.5rem;
    }

    #puzzle {
      display: grid;
      grid-template-columns: repeat(4, 80px);
      grid-template-rows: repeat(4, 80px);
      gap: 5px;
      background: rgba(255, 255, 255, 0.2);
      padding: 10px;
      border-radius: 10px;
    }

    .tile {
      width: 80px;
      height: 80px;
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 1.5rem;
      font-weight: bold;
      background: rgba(255, 255, 255, 0.8);
      color: #1e3a8a;
      border-radius: 8px;
      cursor: pointer;
      transition: 0.2s;
    }

    .tile:hover {
      background: rgba(255, 255, 255, 1);
    }

    .empty {
      background: transparent;
      cursor: default;
    }

    button {
      margin-top: 20px;
      padding: 10px 20px;
      border: none;
      border-radius: 8px;
      background: #10b981;
      color: white;
      font-size: 1rem;
      cursor: pointer;
      transition: 0.3s;
    }

    button:hover {
      background: #059669;
    }
  </style>
</head>
<body>
  <h1>Sliding Puzzle Game</h1>
  <div id="puzzle"></div>
  <button onclick="shuffle()">Shuffle</button>

  <script>
    const size = 4;
    let tiles = [];
    const puzzle = document.getElementById('puzzle');

    function createTiles() {
      tiles = [];
      for (let i = 1; i <= size * size - 1; i++) {
        tiles.push(i);
      }
      tiles.push('');
      render();
    }

    function render() {
      puzzle.innerHTML = '';
      tiles.forEach((num, index) => {
        const tile = document.createElement('div');
        tile.className = 'tile';
        if (num === '') {
          tile.classList.add('empty');
        } else {
          tile.textContent = num;
          tile.onclick = () => move(index);
        }
        puzzle.appendChild(tile);
      });
    }

    function move(index) {
      const emptyIndex = tiles.indexOf('');
      const validMoves = [emptyIndex - 1, emptyIndex + 1, emptyIndex - size, emptyIndex + size];

      // Check if move is valid (same row or column)
      if (
        validMoves.includes(index) &&
        !(emptyIndex % size === 0 && index === emptyIndex - 1) &&
        !(index % size === 0 && emptyIndex === index - 1)
      ) {
        [tiles[emptyIndex], tiles[index]] = [tiles[index], tiles[emptyIndex]];
        render();
        checkWin();
      }
    }

    function shuffle() {
      for (let i = 0; i < 1000; i++) {
        const neighbors = getMovableIndexes();
        const randomMove = neighbors[Math.floor(Math.random() * neighbors.length)];
        move(randomMove);
      }
    }

    function getMovableIndexes() {
      const emptyIndex = tiles.indexOf('');
      const neighbors = [emptyIndex - 1, emptyIndex + 1, emptyIndex - size, emptyIndex + size];
      return neighbors.filter(
        i => i >= 0 && i < tiles.length &&
        !(emptyIndex % size === 0 && i === emptyIndex - 1) &&
        !(i % size === 0 && emptyIndex === i - 1)
      );
    }

    function checkWin() {
      if (tiles.slice(0, -1).every((val, i) => val === i + 1)) {
        alert('🎉 You solved the puzzle!');
      }
    }

    createTiles();
  </script>
</body>
</html>
