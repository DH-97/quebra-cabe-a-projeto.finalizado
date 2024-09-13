
<script lang="ts">

  //Esse import traz um arquivo de CSS global para aplicar estilos que valem para toda a página.
    import "../../styles/global.css"


  // 'grid' é a matriz que representa o estado atual do quebra-cabeça. Cada célula é uma imagem.
  // Essas são as imagens desordenadas do quebra-cabeça.

    let grid = [
        ["part3.png", "part5.png", "part10.png"],
        ["part7.png", "part1.png", "part11.png"],
        ["part4.png", "part8.png", "part9.png"],
        ["part6.png", "part12.png", "part2.png"]
    ]

  // 'correctgrid' é a matriz que mostra a ordem correta das imagens no quebra-cabeça.
    const correctgrid = [
      ["part1.png", "part2.png", "part3.png"],
      ["part4.png", "part5.png", "part6.png"],
      ["part7.png", "part8.png", "part9.png"],
      ["part10.png", "part11.png","part12.png"]
    ];

  // 'selected' guarda a célula que foi clicada (linha e coluna). No começo, é null porque nenhuma célula foi selecionada.

    let selected: any = null
    let ispuzzlesolved: boolean = false


// Quando uma célula é clicada, essa função é chamada.
  // Se o quebra-cabeça já estiver resolvido, a função não faz nada.
    function handleclick(row :number, col : number){
      if (!ispuzzlesolved){
         // Se já tiver uma célula selecionada, troca a posição das duas células clicadas.
        if(selected){
            [grid[row][col], grid[selected.row][selected.col]] = [grid[selected.row][selected.col], grid[row][col]]
            selected = null;  // Desmarca a célula selecionada
            checkIfPuzzleSolved();// Verifica se o quebra-cabeça está resolvido

        }else {
           // Se não tinha célula selecionada, seleciona a célula clicada.
            selected = { row, col };
        }
    }
  } 

    // Verifica se todas as peças estão na posição correta.
    function checkIfPuzzleSolved() {
      ispuzzlesolved = grid.flat().every((cell , index) => {
        const rowIndex = Math.floor(index / 3)
        const colIndex = index % 3;
        return cell === correctgrid[rowIndex][colIndex];
      });
    }


</script>

<style>
    .menu {
      border-radius: 15px;  /* Arredonda os cantos do link de voltar ao menu. */
    }
    table { /* Define o estilo da tabela que organiza o layout do quebra-cabeça. */

      border-collapse: collapse; /* Remove os espaçamentos entre células */
      border-spacing: 0;  /* Remove espaços entre células */
      margin-left: auto;
      margin-right: auto; /* Centraliza a tabela. */
     
    }
    td { /* Define que as células (td) da tabela terão 100x100px e centraliza o conteúdo dentro delas. */
      width: 100px; /* Largura das células. */
      height: 100px; /* Altura das células. */
      
      text-align: center;  /* Alinha o texto (e as imagens) ao centro. */
      vertical-align: middle; /* Alinha verticalmente ao meio. */
      padding: 1,2 px; /* Define o espaçamento interno das células. */
    }
    .selected { /* Aplica uma borda vermelha de 2px nas células selecionadas. */
      border: 2px solid red;
    }
  
    td img {
      width: 110px;  /* Largura fixa */
      height: 110px; /* Altura fixa */
      display: block; /* Remove espaços adicionais em volta da imagem */
      object-fit: cover;  /* Ajusta a imagem para cobrir a célula sem distorcer. */
    }
    .overlay {
      position: fixed;/* Fixa a sobreposição no topo da página. */
      top: 0;
      left: 0;
      width: 100%; /* Preenche toda a largura da tela. */
      height: 100%; /* Preenche toda a altura da tela. */
      background-color: rgba(0, 0, 0, 0.5); /* Fundo semitransparente. */
      display: flex;
      align-items: center; /* Centraliza o conteúdo verticalmente. */
      justify-content: center; /* Centraliza o conteúdo horizontalmente. */
      flex-direction: column; /* Organiza o conteúdo em coluna. */
      color: white; /* Cor do texto. */
      z-index: 100; /* Garante que a sobreposição fique acima dos outros elementos. */
      font-family: "Press Start 2P", system-ui;/* Define a fonte. */
    }
    .hidden {
      display: none;/* Oculta o elemento. */
    }
       /* Especificidade aumentada para garantir que o tamanho da fonte seja aplicado */
       div.overlay h2.congratulations {
        font-size: 1.2rem !important; /* Tamanho da fonte do texto de parabéns. */
        animation: fadeIn 1s ease-in-out forwards; /* Animação de entrada. */
        margin-bottom: 20px;/* Espaço abaixo do título. */
    }
    @keyframes fadeIn {
        0% {
            opacity: 0;
            transform: scale(0.8);/* Começa pequeno e invisível. */
        }
        100% {
          opacity: 1;
          transform: scale(1); /* Termina no tamanho normal e visível. */
        }
      } 

      .next-button {
        padding: 10px 20px; /* Espaço interno do botão. */
        background-color: #28a745;/* Cor de fundo do botão. */
        color: white;/* Cor do texto do botão. */
        border: none; /* Sem borda. */
        border-radius: 5px;/* Borda arredondada. */
        font-size: 1.2rem; /* Tamanho da fonte. */
        cursor: pointer;/* Cursor de ponteiro ao passar o mouse. */
        transition: background-color 0.3s ease;/* Transição suave para a cor de fundo. */
        align-self: center; /* Centraliza o botão dentro do contêiner. */
    }

    .next-button:hover {
        background-color: #360c9f;/* Cor de fundo ao passar o mouse. */
    }
 
  </style>
  
  <a class="jogar" href="/facil"> </a>
  
  <br/>
  <br/>

  <h1>NÍVEL FÁCIL</h1>

  <br/>
  <br/>
  <br/>

<!-- Sobreposição exibida quando o quebra-cabeça está resolvido -->
  {#if ispuzzlesolved}

  <div class="overlay">
    <h2>🎊 Parabéns! Você concluiu o primeiro nível do quebra-cabeça! 🏆</h2>
<p>Passe para o próximo nível 🎯</p>
    <button class="next-button" on:click={() => window.location.href = '/medio'}> PRÓXIMO NÍVEL </button>
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