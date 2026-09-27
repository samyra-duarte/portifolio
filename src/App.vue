<script setup>
import { ref } from 'vue'

// 1. Dados da Artista
const persona = {
  nome: 'Samyra PixelArt',
  cargo: 'Pixel Artist & Game Asset Creator',
  descricao: 'Criando universos em miniatura, focando em paletas vibrantes e animações fluidas para jogos independentes.'
}

// 2. Nova Estrutura Hierárquica (Seções > Subseções > Imagens)
const secoes = ref([
  {
    id: 'jogos',
    titulo: 'JOGOS',
    tipo: 'com_subsecoes',
    subsecoes: [
      {
        id: 'cogutesla',
        titulo: 'Cogutesla',
        descricao: 'Plataforma 2D de ação estrelando um cogumelo robótico tentando sobreviver e navegar por um mundo dominado por máquinas e engrenagens.',
        linkJogar: 'https://itch.io', // Link real entraria aqui
        imagens: ['img1', 'img2', 'img3'] // Mockups de imagens
      }
    ]
  },
  {
    id: 'personagens',
    titulo: 'SPRITES: PERSONAGENS',
    tipo: 'direto_galeria',
    descricao: 'Animações top-down, ciclos de caminhada, ataques e transformações.',
    imagens: ['img1', 'img2', 'img3', 'img4'] // Mockups de imagens
  },
  {
    id: 'ambientes',
    titulo: 'TILESETS: AMBIENTES',
    tipo: 'direto_galeria',
    descricao: 'Cenários, texturas de terreno, vegetação e objetos de cenário.',
    imagens: ['img1', 'img2', 'img3', 'img4', 'img5'] // Mockups de imagens
  }
])

// 3. Lógica do Modal (Foco nas imagens)
const imagemFocada = ref(null)

const abrirModal = (imgIndex) => {
  // Como não temos imagens reais ainda, usamos o index para simular
  imagemFocada.value = `Imagem em destaque`
}

const fecharModal = () => {
  imagemFocada.value = null
}
</script>

<template>
  <main class="pixel-portfolio">
    <!-- Hero Section -->
    <header class="hero-panel">
      <h1 class="pixel-title">{{ persona.nome }}</h1>
      <h2 class="pixel-subtitle">{{ persona.cargo }}</h2>
      <p class="pixel-text">{{ persona.descricao }}</p>
    </header>

    <!-- Loop Principal das Seções -->
    <div class="conteudo-principal">
      <section v-for="secao in secoes" :key="secao.id" class="secao-bloco">
        
        <!-- Título da Seção (Estilo Fita Retrô) -->
        <h2 class="secao-header-title">{{ secao.titulo }}</h2>

        <!-- TIPO 1: SEÇÃO COM SUBSEÇÕES (Ex: Jogos) -->
        <div v-if="secao.tipo === 'com_subsecoes'" class="subsecoes-lista">
          <article v-for="sub in secao.subsecoes" :key="sub.id" class="cartridge">
            
            <div class="cartridge-header">
              <div class="cartridge-info">
                <h3 class="pixel-title-sm">{{ sub.titulo }}</h3>
                <p class="pixel-text-sm">{{ sub.descricao }}</p>
              </div>
              
              <!-- Botão Especial de Jogar -->
              <div class="cartridge-action" v-if="sub.linkJogar">
                <a :href="sub.linkJogar" target="_blank" class="pixel-btn btn-jogar">
                  JOGAR Cogutesla
                </a>
              </div>
            </div>

            <!-- Galeria da Subseção -->
            <div class="galeria-grid">
              <div v-for="(img, idx) in sub.imagens" :key="idx" class="mock-imagem" @click="abrirModal(idx)">
                <span>GIF / PNG</span>
              </div>
            </div>

          </article>
        </div>

        <!-- TIPO 2: GALERIA DIRETA (Ex: Personagens, Ambientes) -->
        <div v-else class="cartridge">
          <div class="cartridge-header" v-if="secao.descricao">
             <p class="pixel-text-sm">{{ secao.descricao }}</p>
          </div>
          <div class="galeria-grid">
            <div v-for="(img, idx) in secao.imagens" :key="idx" class="mock-imagem" @click="abrirModal(idx)">
              <span>GIF / PNG</span>
            </div>
          </div>
        </div>

      </section>
    </div>

    <!-- Modal Escuro (Foco na Imagem) -->
    <div v-if="imagemFocada" class="modal-backdrop" @click="fecharModal">
      <div class="modal-content" @click.stop>
        <button class="fechar-btn" @click="fecharModal">✕</button>
        <div class="mock-imagem-grande">{{ imagemFocada }}</div>
      </div>
    </div>
  </main>
</template>

<style>
/* Importando fonte Pixel Art */
@import url('https://fonts.googleapis.com/css2?family=VT323&family=Silkscreen&display=swap');

:global(body), :global(#app) {
  margin: 0; padding: 0; width: 100%; max-width: 100%;
  background-color: #fcecef;
  background-image: linear-gradient(#f7d5db 1px, transparent 1px), linear-gradient(90deg, #f7d5db 1px, transparent 1px);
  background-size: 32px 32px;
  display: block;
}

.pixel-portfolio {
  --sakura-dark: #7a3e4e;
  --sakura-mid: #e88fa3;
  --sakura-light: #fff5f7;
  --sakura-shadow: #c9798c;
  --btn-special: #7adea0; /* Verde retrô para o botão de jogar */
  --btn-special-shadow: #4a9e69;

  min-height: 100vh;
  padding: 3rem 1.5rem;
  display: flex;
  flex-direction: column;
  align-items: center;
  font-family: 'VT323', monospace;
  color: var(--sakura-dark);
}

.hero-panel {
  background-color: var(--sakura-light);
  border: 4px solid var(--sakura-dark);
  padding: 2rem;
  width: 100%;
  max-width: 900px;
  text-align: center;
  margin-bottom: 4rem;
  box-shadow: 8px 8px 0px var(--sakura-shadow);
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

/* Estrutura de Conteúdo */
.conteudo-principal {
  width: 100%;
  max-width: 900px;
  display: flex;
  flex-direction: column;
  gap: 4rem; /* Espaço grande entre seções principais */
}

.secao-bloco {
  display: flex;
  flex-direction: column;
  gap: 1.5rem;
}

.secao-header-title {
  font-family: 'Silkscreen', cursive;
  font-size: 1.8rem;
  color: var(--sakura-light);
  background-color: var(--sakura-dark);
  margin: 0;
  padding: 0.8rem 1.5rem;
  display: inline-block;
  align-self: flex-start;
  box-shadow: 4px 4px 0px var(--sakura-shadow);
  border: 2px solid var(--sakura-light);
}

/* Cartuchos / Containers */
.subsecoes-lista {
  display: flex;
  flex-direction: column;
  gap: 2rem;
}

.cartridge {
  background-color: var(--sakura-light);
  border: 4px solid var(--sakura-dark);
  padding: 1.5rem;
  box-shadow: 6px 6px 0px var(--sakura-shadow);
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

/* Botões */
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
  background-color: var(--btn-special);
  color: #fff;
  border: 4px solid #1a4a2c;
  box-shadow: 4px 4px 0px var(--btn-special-shadow);
  text-shadow: 2px 2px 0px #1a4a2c;
  font-weight: bold;
}

.btn-jogar:hover { background-color: #8af2b2; }
.btn-jogar:active { box-shadow: 0px 0px 0px var(--btn-special-shadow); transform: translate(4px, 4px); }

/* Galeria Visual */
.galeria-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(180px, 1fr));
  gap: 1rem;
}

/* Quadrados temporários para simular as imagens */
.mock-imagem {
  background-color: #ffdce3;
  border: 2px solid var(--sakura-dark);
  aspect-ratio: 1 / 1;
  display: flex;
  align-items: center;
  justify-content: center;
  color: var(--sakura-dark);
  font-weight: bold;
  cursor: pointer;
  transition: transform 0.2s;
}

.mock-imagem:hover { transform: scale(1.05); background-color: var(--sakura-mid); color: white; }

/* Modal Escuro */
.modal-backdrop {
  position: fixed; top: 0; left: 0; right: 0; bottom: 0;
  background-color: rgba(30, 20, 22, 0.95);
  display: flex; align-items: center; justify-content: center; z-index: 1000; padding: 2rem;
}

.modal-content { position: relative; max-width: 90vw; max-height: 90vh; }

.mock-imagem-grande {
  width: 600px; height: 400px; max-width: 100%; background: var(--sakura-light);
  border: 4px solid var(--sakura-dark); display: flex; align-items: center; justify-content: center;
  font-size: 2rem;
}

.fechar-btn {
  position: absolute; top: -50px; right: 0; background: var(--sakura-light); color: var(--sakura-dark);
  border: 4px solid var(--sakura-dark); width: 40px; height: 40px; font-size: 1.5rem; cursor: pointer;
  display: flex; align-items: center; justify-content: center; font-family: 'Silkscreen';
}

.fechar-btn:hover { background: var(--sakura-mid); color: #fff; }

@media (max-width: 600px) {
  .hero-panel { padding: 1.5rem; }
  .cartridge-header { flex-direction: column; align-items: flex-start; }
  .pixel-btn { width: 100%; text-align: center; }
  .secao-header-title { font-size: 1.5rem; }
}
</style>