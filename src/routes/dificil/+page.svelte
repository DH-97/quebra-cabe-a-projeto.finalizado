
<script lang="ts">

  //Esse import traz um arquivo de CSS global para aplicar estilos que valem para toda a página.
    import "../../styles/global.css"

// A grid é uma matriz onde cada célula é o nome de um arquivo de imagem (tipo "11.png").
  // Essas imagens são as partes do quebra-cabeça.

    let grid = [
        ["11.png", "22.png", "12.png", "7.png","13.png"],
        ["23.png", "5.png", "14.png", "21.png", "6.png"],
        ["4.png", "20.png", "1.png","15.png","24.png"],
        ["19.png", "3.png", "16.png","2.png","9.png"],
        ["18.png", "10.png", "17.png","8.png","25.png"],
    ] 

 // Essa é a grid correta que a gente quer atingir com o quebra-cabeça montado.
    const correctgrid = [
      ["1.png", "2.png", "3.png", "4.png","5.png"],
      ["6.png", "7.png", "8.png", "9.png","10.png"],
      ["11.png", "12.png", "13.png", "14.png","15.png"],
      ["16.png", "17.png", "18.png", "19.png","20.png"],
      ["21.png", "22.png", "23.png" ,"24.png","25.png"],
    ];

// 'selected' guarda a célula que foi clicada (linha e coluna).
  // No começo, não tem célula selecionada, então é null.
    let selected: any = null
    let ispuzzlesolved: boolean = false


 // Quando uma célula é clicada, a função 'handleclick' é chamada.
  // Se o quebra-cabeça já estiver resolvido, não faz nada.
    function handleclick(row :number, col : number){
      if (!ispuzzlesolved){  // bloquei os cliques se o puzzle já foi 
          // Se já tem uma célula selecionada, troca a posição das duas células.
        if(selected){
            [grid[row][col], grid[selected.row][selected.col]] = [grid[selected.row][selected.col], grid[row][col]]
            selected = null;
            checkIfPuzzleSolved();// Verifica se o quebra-cabeça está resolvido
        }else {
          // Se não tinha célula selecionada, seleciona a célula clicada.
            selected = { row, col };
        }
    }
  } 

    // Verifica se o quebra-cabeça está na grid correta.
    function checkIfPuzzleSolved() {
      ispuzzlesolved = grid.flat().every((cell , index) => {
        const rowIndex = Math.floor(index / 5)
        const colIndex = index % 5;
        return cell === correctgrid[rowIndex][colIndex];
      });
    }

</script>

<style>
    .menu {
      border-radius: 15px;/* Arredonda os cantos do link de voltar ao menu. */
    }
    table { /* Define o estilo da tabela que organiza o layout do quebra-cabeça. */
      border-collapse: collapse; /* Remove os espaços entre as células. */
      border-spacing: 0;  /* Remove espaços entre células */
      margin-left: auto;
      margin-right: auto;
     
    }
    td { /* Define que as células (td) da tabela terão 100x100px e centraliza o conteúdo dentro delas. */
      width: 80px; /* Largura das células. */
      height: 80px;/* Altura das células. */
      
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
      display: block; /* Faz com que a imagem ocupe o espaço da célula. */
    }
    .overlay {
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background-color: rgba(0, 0, 0, 0.5); /* Fundo semitransparente. */
      display: flex;
      align-items: center;
      justify-content: center;
      flex-direction: column;
      color: white;
      z-index: 100; /* Faz com que o overlay fique acima dos outros elementos. */
      font-family: "Press Start 2P", system-ui;
    }
    .hidden {
      display: none; /* Oculta o elemento. */
    }
       /* Especificidade aumentada para garantir que o tamanho da fonte seja aplicado */
       div.overlay h2.congratulations {
        font-size: 1.2rem !important; /* Diminui o tamanho da fonte e força a aplicação */
        animation: fadeIn 1s ease-in-out forwards;/* Animação de entrada. */
        margin-bottom: 20px; /* Espaço abaixo do título. */
    }
    @keyframes fadeIn {
        0% {
            opacity: 0;
            transform: scale(0.8);/* Começa com o título menor e invisível. */
        }
        100% {
          opacity: 1;
          transform: scale(1);/* Termina com o título em tamanho normal e visível. */
        }
      } 

      .next-button {
        padding: 10px 20px;/* Espaço interno do botão. */
        background-color: #28a745; /* Cor de fundo do botão. */
        color: white;/* Cor do texto do botão. */
        border: none;
        border-radius: 5px; /* Borda arredondada. */
        font-size: 1.2rem;/* Tamanho da fonte. */
        cursor: pointer;/* Mostra o cursor de ponteiro ao passar o mouse. */
        transition: background-color 0.3s ease; /* Transição suave para a cor de fundo. */
        align-self: center;/* Centraliza o botão. */
    }

    .next-button:hover {
        background-color: #360c9f;/* Cor de fundo quando o botão é hover. */
    }
 
  </style>
  
  <a class="jogar" href="/dificil"> </a>

  <br/>
  <br/>

  <h1>NÍVEL DifIcil</h1>

  <br/>
  <br/>
  <br/>


<!-- Sobreposição exibida quando o quebra-cabeça está resolvido -->
  {#if ispuzzlesolved}

  <div class="overlay">
    <h2>🎉 Parabéns! Você é incrível, conseguiu completar todas as fases do jogo! 🏅</h2>
    <p>Obrigado por jogar! 🎯</p>
    <button class="next-button" on:click={() => window.location.href = '/'}>fINALIZAR JOGO</button>
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