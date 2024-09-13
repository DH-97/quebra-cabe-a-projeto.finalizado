
<script lang="ts">

    import "../../styles/global.css"


    // Grid inicial com as imagens das peças do quebra-cabeça
    let grid = [
        ["-14.png","-13.png","-4.png","-15.png"],
        ["-5.png","-3.png","-7.png","-16.png"],
        ["-10.png","-12.png","-2.png","-6.png"],
        ["-8.png","-9.png","-11.png","-1.png"],
        
    ]
  // Grid com a disposição correta das peças para comparação
    const correctgrid = [
      ["-1.png","-2.png","-3.png","-4.png"],
      ["-5.png","-6.png","-7.png","-8.png"],
      ["-9.png","-10.png","-11.png","-12.png"],
      ["-13.png","-14.png","-15.png","-16.png"]
    ];
// Variável para armazenar a célula selecionada
    let selected: any = null
     // Variável para verificar se o quebra-cabeça está resolvido
    let ispuzzlesolved: boolean = false

    // Função chamada quando uma célula é clicada
    function handleclick(row :number, col : number){
      if (!ispuzzlesolved){  // bloquei os cliques se o puzzle já foi resolvido
        if(selected){
            // Troca as peças entre a célula selecionada e a célula clicada
            [grid[row][col], grid[selected.row][selected.col]] = [grid[selected.row][selected.col], grid[row][col]]
            selected = null;
            checkIfPuzzleSolved();// Verifica se o quebra-cabeça foi resolvido
        }else {
            selected = { row, col }; // Marca a célula clicada como selecionada
        }
    }
  } 

     // Função que verifica se o quebra-cabeça está resolvido comparando com a solução correta
    function checkIfPuzzleSolved() {
      ispuzzlesolved = grid.flat().every((cell , index) => {
        const rowIndex = Math.floor(index / 4)
        const colIndex = index % 4;
        return cell === correctgrid[rowIndex][colIndex];
      });
    }

</script>

<style>
    .menu {
      border-radius: 15px;  /* Arredonda os cantos do link de voltar para o menu */
    }
    table { /* Define o estilo da tabela que organiza o layout do quebra-cabeça. */
      border-collapse: collapse; /* Remove os espaçamentos entre células */
      border-spacing: 0;  /* Remove espaços entre células */
      margin-left: auto;
      margin-right: auto;
     
    }
    td { /* Define que as células (td) da tabela terão 100x100px e centraliza o conteúdo dentro delas. */
      width: 80px;
      height: 80px;
      
      text-align: center;
      vertical-align: middle;
      padding:  1px; /* Remove o padding das células */
    }
    .selected { /* Aplica uma borda vermelha de 2px nas células selecionadas. */
      border: 2px solid red;
    }
    
    td img {
      width: 100px;  /* Largura fixa */
      height: 120px; /* Altura fixa */      
      display: block; /* Remove espaços adicionais em volta da imagem */
    }
    .overlay {
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background-color: rgba(0, 0, 0, 0.5); /* Fundo escurecido para a sobreposição */
      display: flex;
      align-items: center;
      justify-content: center;
      flex-direction: column;
      color: white;
      z-index: 100;
      font-family: "Press Start 2P", system-ui;
    }
    .hidden {
      display: none;  /* Esconde elementos com a classe 'hidden' */
    }
       /* Especificidade aumentada para garantir que o tamanho da fonte seja aplicado */
       div.overlay h2.congratulations {
        font-size: 1.2rem !important;  /* Ajusta o tamanho da fonte do título */
        animation: fadeIn 1s ease-in-out forwards;/* Animação de fade-in para o título */
        margin-bottom: 20px;
    }
    @keyframes fadeIn {
        0% {
            opacity: 0;
            transform: scale(0.8);
        }
        100% {
          opacity: 1;
          transform: scale(1);
        }
      } 

      .next-button {
        padding: 10px 20px;
        background-color: #28a745;
        color: white;
        border: none;
        border-radius: 5px;/* Arredonda os cantos do botão */
        font-size: 1.2rem;
        cursor: pointer;
        transition: background-color 0.3s ease; /* Transição suave para a cor de fundo */
        align-self: center; /* Garante que o botão esteja no centro */
    }

    .next-button:hover {
        background-color: #360c9f;/* Cor de fundo do botão quando o mouse está sobre ele */
      }
    
 
  </style>
  
  <a class="jogar" href="/dificil"> </a>

  
  <br/>
  <br/>

  <h1>NÍVEL MÉDIO</h1>

  <br/>
  <br/>
  <br/>

<!-- Sobreposição exibida quando o quebra-cabeça está resolvido -->
  {#if ispuzzlesolved}

  <div class="overlay">
    <h2>🎊 Muito bom! Você conseguiu concluir o segundo nível do quebra-cabeça! 🏆🎯</h2>
    <p>Passe para o próximo nível</p>    
    <button class="next-button" on:click={() => window.location.href = '/dificil'}>PRÓXIMO NIVEL</button>
  </div>
  
  {/if}
  
  <table>
    
    {#each grid as row, rowIndex}
      <tr>
        {#each row as cell, colIndex}
          <td
            class="cell {selected && selected.row === rowIndex && selected.col === colIndex ? 'selected' : ''}"
            on:click={() =>handleclick(rowIndex, colIndex)}
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

  <!-- Link para voltar ao menu -->
  <a class="menu" href="/jogar">VOLTAR</a>
  
