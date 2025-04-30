<script>
  import { onMount } from 'svelte';
  
  const frutas = ['🍎', '🍊', '🍇', '🍒', '🍋', '🍉', '🍓'];
  let reels = ['', '', ''];
  let spinning = false;
  let saldo = 100;
  let aposta = 10;
  let frutasVisiveis = ['', '', ''];
  
  function gerarSequencia() {
    const sequencia = [];
    // Gera 12 frutas para cada rolo
    for (let i = 0; i < 12; i++) {
      sequencia.push(frutas[Math.floor(Math.random() * frutas.length)]);
    }
    return sequencia;
  }
  
  function girar() {
    if (spinning || saldo < aposta) return;
    
    spinning = true;
    saldo -= aposta;
    
    // Gera resultados finais aleatórios para cada rolo
    reels = reels.map(() => frutas[Math.floor(Math.random() * frutas.length)]);
    
    // Inicia a animação de mudança rápida de frutas
    let contador = 0;
    const intervalo = setInterval(() => {
      // Atualiza as frutas visíveis aleatoriamente
      frutasVisiveis = frutasVisiveis.map(() => frutas[Math.floor(Math.random() * frutas.length)]);
      contador++;
      
      // Para após 20 iterações e mostra o resultado final
      if (contador >= 20) {
        clearInterval(intervalo);
        frutasVisiveis = reels;
        spinning = false;
        verificarGanhador();
      }
    }, 100);
  }
  
  function verificarGanhador() {
    if (reels[0] === reels[1] && reels[1] === reels[2]) {
      const premio = aposta * 3;
      saldo += premio;
      alert(`Parabéns! Você ganhou ${premio} moedas!`);
    }
  }
</script>

<div class="slot-machine">
  <div class="cabecalho">
    <div class="saldo">Saldo: {saldo} moedas</div>
    <div class="aposta">
      <div class="valor-aposta">Aposta: {aposta}</div>
      <div class="botoes-aposta">
        <button on:click={() => aposta = Math.max(1, aposta - 1)}>-</button>
        <button on:click={() => aposta = Math.min(saldo, aposta + 1)}>+</button>
      </div>
    </div>
  </div>
  
  <div class="maquina">
    <div class="visor">
      <div class="frame"></div>
      <div class="reels">
        {#each frutasVisiveis as fruta, i}
          <div class="reel">
            <div class="simbolo">{fruta}</div>
          </div>
        {/each}
      </div>
    </div>
    <div class="base"></div>
    <button class="girar" on:click={girar} disabled={spinning || saldo < aposta}>
      {spinning ? 'Girando...' : 'Girar'}
    </button>
  </div>
</div>

<style>
  .slot-machine {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 20px;
    padding: 20px;
    background: #2c3e50;
    border-radius: 10px;
    color: white;
    box-shadow: 0 0 20px rgba(0, 0, 0, 0.3);
  }
  
  .cabecalho {
    display: flex;
    justify-content: space-between;
    width: 100%;
    padding: 0 20px;
  }
  
  .saldo {
    font-size: 1.5em;
    font-weight: bold;
    color: #f1c40f;
  }
  
  .aposta {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 5px;
  }
  
  .botoes-aposta {
    display: flex;
    gap: 10px;
  }
  
  .botoes-aposta button {
    width: 30px;
    height: 30px;
    padding: 0;
    display: flex;
    align-items: center;
    justify-content: center;
    background: #34495e;
  }
  
  .maquina {
    position: relative;
    width: 300px;
    height: 200px;
    background: #34495e;
    border-radius: 10px;
    padding: 20px;
  }
  
  .visor {
    background: #2c3e50;
    border: 5px solid #95a5a6;
    border-radius: 5px;
    padding: 10px;
    height: 100px;
    display: flex;
    align-items: center;
    justify-content: center;
    position: relative;
  }
  
  .frame {
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    border: 2px solid #e74c3c;
    pointer-events: none;
    z-index: 2;
  }
  
  .reels {
    display: flex;
    gap: 10px;
    width: 100%;
    height: 100%;
  }
  
  .reel {
    flex: 1;
    display: flex;
    align-items: center;
    justify-content: center;
    background: #ecf0f1;
    border-radius: 5px;
    overflow: hidden;
  }
  
  .simbolo {
    font-size: 3em;
    text-align: center;
    width: 100%;
    height: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
  }
  
  .base {
    position: absolute;
    bottom: 0;
    left: 0;
    right: 0;
    height: 20px;
    background: #95a5a6;
    border-radius: 0 0 10px 10px;
  }
  
  .girar {
    position: absolute;
    bottom: 30px;
    right: 20px;
    padding: 10px 20px;
    font-size: 1.2em;
    background: #e74c3c;
    color: white;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    transition: transform 0.2s;
  }
  
  .girar:hover:not(:disabled) {
    transform: scale(1.05);
  }
  
  .girar:disabled {
    background: #95a5a6;
    cursor: not-allowed;
  }
  
  .valor-aposta {
    font-size: 1.2em;
    margin-bottom: 5px;
  }
</style> 