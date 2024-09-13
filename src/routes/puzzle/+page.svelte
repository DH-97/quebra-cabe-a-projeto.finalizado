<script lang="ts">
    import "../../styles/global.css"

    let grid = [
        [null , null , null],
        [null , null , null],
        ["puzzle.png", "puzzle.png", "puzzle.png"]
    ]

    let selected = null

    function handleclick(row :number, col : number){
        if(selected){
            [grid[row][col], grid[selected.row][selected.col]] = [grid[selected.row][selected.col], grid[row][col]]
            selected = null;
        }else {
            selected = { row, col };
        }
    }
</script>
<style>
    .menu {
      border-radius: 15px;
    }
    table {
      border-collapse: collapse;
    }
    td {
      width: 100px;
      height: 100px;
      border: 1px solid #000;
      text-align: center;
      vertical-align: middle;
    }
    .selected {
      border: 2px solid red;
    }
  </style>
  
  <a class="jogar" href="/facil"> </a>
  <h1>NÍVEL FÁCIL</h1>
  
  <table>
    {#each grid as row, rowIndex}
      <tr>
        {#each row as cell, colIndex}
          <td
            class="cell {selected && selected.row === rowIndex && selected.col === colIndex ? 'selected' : ''}"
            on:click={() => handleClick(rowIndex, colIndex)}
          >
            {#if cell}
              <img src={cell} alt="img" />
            {/if}
          </td>
        {/each}
      </tr>
    {/each}
  </table>
  
  <br/>
  <br/>
  <br/>
  <a class="menu" href="/jogar">VOLTAR</a>