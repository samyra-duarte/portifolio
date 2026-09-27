<template>
  <main class="pixel-portfolio">
    <!-- Dados Pessoa -->
    <header class="hero-panel">
      <h1 class="pixel-title">{{ persona.nome }}</h1>
      <h2 class="pixel-subtitle">{{ persona.cargo }}</h2>
      <p class="pixel-text">{{ persona.descricao }}</p>
    </header>

    <!-- Loop Principal das Seções -->
    <div class="conteudo-principal">
      <section v-for="secao in secoes" :key="secao.id" class="secao-bloco">
        
        <!-- Título da Seção -->
        <h2 class="secao-header-title">{{ secao.titulo }}</h2>

        <!-- TIPO 1: SEÇÃO COM SUBSEÇÕES (Ex: Jogos) -->
        <div v-if="secao.tipo === 'com_subsecoes'" class="subsecoes-lista">
          <article v-for="sub in secao.subsecoes" :key="sub.id" class="cartridge">
            
            <div class="cartridge-header">
              <div class="cartridge-info">
                <h3 class="pixel-title-sm">{{ sub.titulo }}</h3>
                <p class="pixel-text-sm">{{ sub.descricao }}</p>
              </div>
              
              <div class="cartridge-action" v-if="sub.linkJogar">
                <a :href="sub.linkJogar" target="_blank" class="pixel-btn btn-jogar">
                  JOGAR {{ sub.titulo }}
                </a>
              </div>
            </div>

            <div class="galeria-grid">
              <div v-for="(img, idx) in sub.imagens" :key="idx" class="mock-imagem" @click="abrirModal(img)">
                <img :src="img" :alt="sub.titulo" class="pixel-art-img" />
              </div>
            </div>

          </article>
        </div>

        <!-- TIPO 2: GALERIA DIRETA -->
        <div v-else class="cartridge">
          <div class="cartridge-header" v-if="secao.descricao">
             <p class="pixel-text-sm">{{ secao.descricao }}</p>
          </div>
          
          <div class="galeria-grid">
            <div v-for="(img, idx) in secao.imagens" :key="idx" class="mock-imagem" @click="abrirModal(img)">
              <img :src="img" :alt="secao.titulo" class="pixel-art-img" />
            </div>
          </div>
        </div>

      </section>
    </div>

    <!-- MODAL ATUALIZADO (Efeito Lightbox Imersivo) -->
    <div v-if="imagemFocada" class="modal-backdrop" @click="fecharModal">
      
      <!-- Botão de Fechar fixo no topo direito -->
      <button class="fechar-btn" @click.stop="fecharModal" title="Fechar">✕</button>

      <!-- Controles de Zoom Overlay flutuando na base -->
      <div class="zoom-controls" @click.stop>
        <button class="zoom-btn" @click="alterarZoom(-0.5)" title="Diminuir Zoom">🔎 -</button>
        <span class="zoom-display">{{ Math.round(escalaZoom * 100) }}%</span>
        <button class="zoom-btn" @click="alterarZoom(0.5)" title="Aumentar Zoom">🔎 +</button>
        <button class="zoom-btn btn-reset" @click="resetarZoom()" title="Tamanho Original">1:1</button>
      </div>

      <!-- Container invisível apenas para segurar a imagem centralizada -->
      <div class="modal-image-container" @click.stop>
        <img 
          :src="imagemFocada" 
          class="modal-img-grande" 
          :style="{ transform: `scale(${escalaZoom})` }"
          alt="Arte em destaque" 
        />
      </div>
      
    </div>
  </main>
</template>

<script setup>
import { ref } from 'vue'

const persona = {
  nome: 'Samyra PixelArt',
  cargo: 'Pixel Artist & Game Asset Creator',
  descricao: 'Criando universos em miniatura com pequenos pixels.'
}

const secoes = ref([
  {
    id: 'jogos',
    titulo: 'JOGOS',
    tipo: 'com_subsecoes',
    subsecoes: [
      {
        id: 'cogutesla',
        titulo: 'Cogutesla',
        descricao: 'Plataforma 2D de ação com um cogumelo simbiótico tentando sobreviver e navegar por um mundo dominado por máquinas e engrenagens.',
        linkJogar: 'https://spinoshroom.itch.io/cogutesla',
        imagens: [
        '/cogutesla/cogutesla1.png',
        '/cogutesla/cogutesla2.png',  
        '/cogutesla/cogutesla3.png',
        '/cogutesla/cogutesla4.png',
        '/cogutesla/cogutesla5.png',
        '/cogutesla/cogutesla6.png',
        '/cogutesla/cogutesla7.png',
        '/cogutesla/cogutesla8.png',
        ]
      }
    ]
  },
  {
    id: 'personagens',
    titulo: 'SPRITES',
    tipo: 'direto_galeria',
    descricao: 'Animações top-down, ciclos de caminhada, ataques e transformações.',
    imagens: [
      'personagens/personagem_jogavel.png',
      'personagens/personagem_jogavel_2c.gif',
      'personagens/personagem_jogavel_4c.gif',
      'personagens/Bolhas_Goticas.png',
    ]
  },
  {
    id: 'ambientes',
    titulo: 'TILESETS: AMBIENTES',
    tipo: 'direto_galeria',
    descricao: 'Cenários, texturas de terreno, vegetação e objetos de cenário.',
    imagens: [
      'ambientes/florestarpg.png',
    ]
  }
])

// Lógica do Modal Imersivo
const imagemFocada = ref(null)
const escalaZoom = ref(1) 

const abrirModal = (caminhoImg) => {
  imagemFocada.value = caminhoImg
  escalaZoom.value = 2 
}

const fecharModal = () => {
  imagemFocada.value = null
  escalaZoom.value = 2
}

const alterarZoom = (fator) => {
  const novaEscala = escalaZoom.value + fator
  if (novaEscala >= 0.5 && novaEscala <= 5) {
    escalaZoom.value = novaEscala
  }
}

const resetarZoom = () => {
  escalaZoom.value = 1
}
</script>

<style>
/* Importando fonte Pixel Art */
@import url('https://fonts.googleapis.com/css2?family=VT323&family=Silkscreen&display=swap');

:global(body), :global(#app) {
  margin: 0; padding: 0; width: 100%; max-width: 100%;
  background-color: #FDF5E6;
  display: block;
}

.pixel-portfolio {
  /* Novas Cores do Tema Floral/Retrô */
  --bg-creme: #FAF5EB;
  --card-bg: #FBEFDE;
  --borda-marrom: #4A2B29;
  --sombra-rosa: #D9959E;
  --verde-musgo: #38513A;
  --verde-borda: #213527;
  --verde-sombra: #2D4334;
  --dourado: #D99C54;

  /* Aplicação das variáveis base */
  min-height: 100vh;
  padding: 3rem 1.5rem;
  display: flex;
  flex-direction: column;
  align-items: center;
  font-family: 'VT323', monospace;
  color: var(--borda-marrom);
}

.hero-panel {
  background-color: var(--card-bg);
  border: 4px solid var(--borda-marrom);
  padding: 2rem;
  width: 100%;
  max-width: 900px;
  text-align: center;
  margin-bottom: 4rem;
  box-shadow: 8px 8px 0px var(--sombra-rosa);
}

.pixel-title {
  font-family: 'Silkscreen', cursive;
  font-size: 2.5rem;
  color: var(--sakura-mid);
  margin: 0 0 1rem 0;
  text-shadow: 2px 2px 0px var(--sakura-dark);
}

.pixel-subtitle { font-size: 1.5rem; margin: 0 0 1rem 0; }
.pixel-text { font-family: 'Segoe UI', sans-serif; font-size: 1.1rem; font-weight: 500; line-height: 1.6; margin: 0; color: #5c2f3b; }

.conteudo-principal {
  width: 100%;
  max-width: 900px;
  display: flex;
  flex-direction: column;
  gap: 4rem;
}

.secao-bloco {
  display: flex;
  flex-direction: column;
  gap: 1.5rem;
}

.secao-header-title {
  font-family: 'Silkscreen', cursive;
  font-size: 1.8rem;
  color: #FDF9ED;
  background-color: var(--verde-musgo);
  margin: 0;
  padding: 0.8rem 1.5rem;
  display: inline-block;
  align-self: flex-start;
  border: 4px solid var(--verde-borda);
  box-shadow: 4px 4px 0px var(--sombra-rosa);
}

.subsecoes-lista {
  display: flex;
  flex-direction: column;
  gap: 2rem;
}

.cartridge {
  background-color: var(--card-bg);
  border: 4px solid var(--borda-marrom);
  padding: 1.5rem;
  box-shadow: 6px 6px 0px var(--sombra-rosa);
}

.cartridge-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 1.5rem;
  padding-bottom: 1rem;
  border-bottom: 4px dashed var(--sakura-shadow);
  flex-wrap: wrap;
  gap: 1rem;
}

.cartridge-info { flex: 1; min-width: 250px; }
.pixel-title-sm { font-size: 2rem; margin: 0 0 0.5rem 0; color: var(--sakura-dark); font-weight: bold; }
.pixel-text-sm { font-family: 'Segoe UI', sans-serif; font-size: 1rem; font-weight: 500; margin: 0; }

.pixel-btn {
  font-family: 'VT323', monospace;
  font-size: 1.3rem;
  text-decoration: none;
  display: inline-block;
  padding: 0.6rem 1.2rem;
  cursor: pointer;
  transition: all 0.1s ease;
}

.btn-jogar {
  background-color: var(--verde-musgo);
  color: #FDF9ED;
  border: 4px solid var(--verde-borda);
  box-shadow: 4px 4px 0px var(--sombra-rosa);
  text-shadow: 2px 2px 0px #1a4a2c;
  font-weight: bold;
}

.btn-jogar:hover { background-color: #56a072; }
.btn-jogar:active { box-shadow: 0px 0px 0px var(--btn-special-shadow); transform: translate(4px, 4px); }

.galeria-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(180px, 1fr));
  gap: 1rem;
}

.mock-imagem {
  background-color: black;
  border: 2px solid var(--sakura-dark);
  aspect-ratio: 1 / 1;
  display: flex;
  overflow: hidden; 
  cursor: pointer;
  transition: transform 0.2s, box-shadow 0.2s;
}

.mock-imagem:hover { 
  transform: scale(1.05) translateY(-5px); 
  box-shadow: 0px 5px 0px var(--sakura-shadow); 
}

.pixel-art-img {
  width: 100%;
  height: 100%;
  object-fit: cover; 
  
  image-rendering: -moz-crisp-edges;
  image-rendering: -webkit-optimize-contrast;
  image-rendering: crisp-edges;
  image-rendering: pixelated; 
}

/* =========================================================
   ESTILOS DO MODAL LIGHTBOX IMERSIVO
   ========================================================= */

.modal-backdrop {
  position: fixed; 
  top: 0; left: 0; right: 0; bottom: 0;
  /* Fundo quase 100% escuro, usando um tom bem denso do verde-musgo para manter coesão */
  background-color: rgba(15, 15, 15, 0.95);
  display: flex; 
  align-items: center; 
  justify-content: center; 
  z-index: 1000; 
}

.modal-image-container {
  width: 100vw;
  height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  overflow: hidden; 
}

.modal-img-grande {
  max-width: 90vw; 
  max-height: 90vh; 
  object-fit: contain; 
  transition: transform 0.2s ease-out; 
  transform-origin: center center;
  
  image-rendering: -moz-crisp-edges;
  image-rendering: -webkit-optimize-contrast;
  image-rendering: crisp-edges;
  image-rendering: pixelated; 
}

.zoom-controls {
  position: absolute;
  bottom: 2rem;
  display: flex;
  align-items: center;
  gap: 1rem;
  /* Fundo sólido do card para garantir total legibilidade */
  background: var(--card-bg);
  border: 4px solid var(--borda-marrom);
  border-radius: 0; /* Removido border-radius para manter a estética em blocos (pixel art) */
  padding: 0.5rem 1rem;
  box-shadow: 4px 4px 0px var(--verde-sombra);
  z-index: 1001; 
}

.zoom-display {
  font-family: 'VT323', monospace;
  font-size: 1.8rem;
  color: var(--borda-marrom);
  min-width: 4rem;
  text-align: center;
}

.zoom-btn {
  font-family: 'Press Start 2P', 'Silkscreen', cursive; /* Usa a fonte em blocos se disponível */
  font-size: 1.2rem;
  background: var(--verde-musgo);
  color: #FDF9ED;
  border: 2px solid var(--verde-borda);
  cursor: pointer;
  padding: 0.5rem 0.8rem;
  transition: all 0.1s;
  box-shadow: 2px 2px 0px var(--verde-sombra);
}

.zoom-btn:hover { 
  background: #4a6d55; /* Um tom um pouco mais claro do verde-musgo */
  transform: translate(-1px, -1px);
  box-shadow: 3px 3px 0px var(--verde-sombra);
}

.zoom-btn:active {
  transform: translate(2px, 2px);
  box-shadow: 0px 0px 0px var(--verde-sombra);
}
.btn-reset { font-family: 'VT323', monospace; font-size: 1.5rem; }

/* Botão Fechar Solto no Canto da Tela (Correção de Contraste) */
.fechar-btn {
  position: absolute; 
  top: 2rem; 
  right: 2rem; 
  background: var(--card-bg); 
  color: var(--borda-marrom);
  border: 4px solid var(--borda-marrom); 
  border-radius: 0; /* Estética de bloco */
  width: 50px; 
  height: 50px; 
  font-size: 1.8rem; 
  cursor: pointer;
  display: flex; 
  align-items: center; 
  justify-content: center; 
  font-family: 'Silkscreen', cursive;
  z-index: 1001;
  box-shadow: 4px 4px 0px var(--sombra-rosa);
  transition: all 0.1s;
}

.fechar-btn:hover { 
  background: var(--verde-musgo); 
  color: #FDF9ED; 
  border-color: var(--verde-borda);
}

.fechar-btn:active {
  transform: translate(4px, 4px);
  box-shadow: 0px 0px 0px var(--verde-sombra);
}

@media (max-width: 600px) {
  /* Reduz o espaçamento geral da página */
  .pixel-portfolio { padding: 1.5rem 1rem; }
  .conteudo-principal { gap: 2.5rem; }
  
  /* Compacta os painéis */
  .hero-panel { padding: 1.5rem 1rem; margin-bottom: 2.5rem; }
  .cartridge { padding: 1rem; }
  .cartridge-header { flex-direction: column; align-items: flex-start; gap: 0.8rem; }

  /* Escala a tipografia para o mobile */
  .pixel-title { font-size: 1.8rem; }
  .pixel-subtitle { font-size: 1.2rem; }
  .pixel-text { font-size: 1rem; }
  .secao-header-title { font-size: 1.3rem; padding: 0.5rem 1rem; }
  .pixel-title-sm { font-size: 1.4rem; }
  .pixel-text-sm { font-size: 0.95rem; line-height: 1.4; }

  /* 4. Força o Grid a manter 2 colunas para imagens não ficarem colossais */
  .galeria-grid { 
    grid-template-columns: repeat(2, 1fr); 
    gap: 0.5rem; 
  }

  /* Ajusta botões e UI do modal */
  .pixel-btn { width: 100%; text-align: center; font-size: 1.1rem; padding: 0.5rem; }
  .fechar-btn { top: 1rem; right: 1rem; width: 40px; height: 40px; font-size: 1.2rem;}
  .zoom-controls { bottom: 1rem; padding: 0.3rem 0.5rem; gap: 0.5rem; }
  .zoom-btn { padding: 0.3rem 0.5rem; font-size: 1rem;}
  .zoom-display { font-size: 1.2rem; min-width: 3rem;}
}
</style>