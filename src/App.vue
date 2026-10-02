<template>
  <main class="pixel-portfolio">
    
    <!-- HUD Superior Esquerdo: Botão de Toggle do CRT -->
    <button class="btn-crt-toggle" @click="crtAtivo = !crtAtivo" :title="crtAtivo ? 'Desligar Efeito CRT' : 'Ligar Efeito CRT'">
      EFEITO CRT: {{ crtAtivo ? 'ON' : 'OFF' }}
    </button>

    <!-- HUD Superior Direito: Redes Sociais -->
    <div class="social-hud">
      <template v-for="rede in redesSociais" :key="rede.id">
        <a 
          v-if="rede.ativo" 
          :href="rede.link" 
          target="_blank" 
          class="pixel-btn social-btn"
          :class="rede.id"
          :title="rede.nome"
        >
          {{ rede.icone }}
        </a>
      </template>
    </div>

    <!-- Overlay de scanlines -->
    <div v-if="crtAtivo" class="crt-overlay"></div>

    <!-- Dados Pessoa -->
    <header class="hero-panel">
      <div class="hero-inner">
        <h1 class="pixel-title">{{ persona.nome }}</h1>
        <div class="divisor"></div>
        <h2 class="pixel-subtitle">{{ persona.cargo }}</h2>
      </div>
    </header>

    <!-- Loop Principal das Seções -->
    <div class="conteudo-principal">
      <section v-for="secao in secoes" :key="secao.id" class="secao-bloco">
        
        <div class="secao-header-wrapper">
          <h2 class="secao-header-title">{{ secao.titulo }}</h2>
        </div>

        <!-- TIPO 1: SEÇÃO COM SUBSEÇÕES (Ex: Jogos) -->
        <div v-if="secao.tipo === 'com_subsecoes'" class="subsecoes-lista">
          <article v-for="sub in secao.subsecoes" :key="sub.id" class="cartridge">
            
            <div class="cartridge-header">
              <div class="cartridge-info">
                <h3 class="pixel-title-sm">{{ sub.titulo }}</h3>
                <p class="pixel-text-sm">{{ sub.descricao }}</p>
              </div>
              
              <div class="cartridge-action" v-if="mostrarBotaoJogar(sub.linkJogar)">
                <a :href="sub.linkJogar" target="_blank" class="pixel-btn btn-jogar">
                  <span class="btn-icon">▶</span> JOGAR
                </a>
              </div>
            </div>

            <div class="galeria-grid">
              <div v-for="(img, idx) in sub.imagens" :key="idx" class="mock-imagem" @click="abrirModal(img)">
                <div class="image-overlay"></div>
                <img :src="img" :alt="sub.titulo" class="pixel-art-img" />
              </div>
            </div>

          </article>
        </div>

        <!-- TIPO 2: GALERIA DIRETA -->
        <div v-else class="cartridge">
          <div class="cartridge-header" v-if="secao.descricao">
             <p class="pixel-text-sm destaque-texto">{{ secao.descricao }}</p>
          </div>
          
          <div class="galeria-grid">
            <div v-for="(img, idx) in secao.imagens" :key="idx" class="mock-imagem" @click="abrirModal(img)">
              <div class="image-overlay"></div>
              <img :src="img" :alt="secao.titulo" class="pixel-art-img" />
            </div>
          </div>
        </div>

      </section>
    </div>

    <!-- MODAL ATUALIZADO (Sem interferência do CRT) -->
    <transition name="fade">
      <div v-if="imagemFocada" class="modal-backdrop" @click="fecharModal">
        
        <!-- Botão de Fechar fixo no topo direito -->
        <button class="fechar-btn" @click.stop="fecharModal" title="Fechar">✕</button>

        <!-- Controles de Zoom Overlay flutuando na base -->
        <div class="zoom-controls" @click.stop>
          <button class="zoom-btn" @click="alterarZoom(-0.5)" title="Diminuir Zoom">-</button>
          <span class="zoom-display">{{ Math.round(escalaZoom * 100) }}%</span>
          <button class="zoom-btn" @click="alterarZoom(0.5)" title="Aumentar Zoom">+</button>
          <div class="divisor-vertical"></div>
          <button class="zoom-btn btn-reset" @click="resetarZoom()" title="Tamanho Original">1:1</button>
        </div>

        <!-- Container da imagem -->
        <div class="modal-image-container" @click.stop>
          <img 
            :src="imagemFocada" 
            class="modal-img-grande" 
            :style="{ width: `${50 * escalaZoom}vw`, height: `${45 * escalaZoom}vh` }"
            alt="Arte em destaque" 
          />
        </div>
        
      </div>
    </transition>
  </main>
</template>

<script setup>
import { ref } from 'vue'

const persona = {
  nome: 'Samyra PixelArt',
  cargo: 'Pixel Artist & Game Asset Creator',
  descricao: 'Criando universos em miniatura com pequenos pixels.'
}

// Configuração das Redes Sociais
// Para ativar uma rede futura, basta mudar "ativo: false" para "ativo: true" e preencher o link
const redesSociais = ref([
  { id: 'wpp', nome: 'WhatsApp', icone: 'WhatsApp', link: 'https://wa.me/553784025322', ativo: true },
  { id: 'ig', nome: 'Instagram', icone: 'IG', link: 'https://instagram.com/seu_user', ativo: false },
  { id: 'in', nome: 'LinkedIn', icone: 'IN', link: 'https://linkedin.com/in/seu_user', ativo: false },
  { id: 'x', nome: 'Twitter/X', icone: 'X', link: 'https://x.com/seu_user', ativo: false }
])

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
        'cogutesla/cogutesla2.png',  
        'cogutesla/cogutesla3.png',
        'cogutesla/cogutesla4.png',
        'cogutesla/cogutesla5.png',
        'cogutesla/cogutesla6.png',
        'cogutesla/cogutesla7.png',
        'cogutesla/cogutesla8.png',
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
      'personagens/bolha_brilhante.gif',
      'personagens/bolha_gotica.gif',
    ]
  },
  {
    id: 'ambientes-isaac',
    titulo: 'CENÁRIOS & TILESETS',
    tipo: 'com_subsecoes',
    subsecoes: [
      {
        id: 'basement-isaac-fan-game',
        titulo: 'Subsolo (The Binding of Isaac)',
        descricao: 'Tileset 16x16 inspirada em The Binding of Isaac. Inclui autotile de parede, variações de piso com rachaduras/manchas, pedras e portas com tranca e abertas.',
        imagens: [
          'ambientes/isaac/0.png',  // 1º: A sala montada em jogo
          'ambientes/isaac/1.png',  // 1º: A sala montada em jogo
          'ambientes/isaac/5.png',  // 1º: A sala montada em jogo
          'ambientes/isaac/2.png',  // 1º: A sala montada em jogo
          'ambientes/isaac/3.png',  // 1º: A sala montada em jogo
          'ambientes/isaac/4.png',  // 1º: A sala montada em jogo
          'ambientes/isaac/6.png',  // 1º: A sala montada em jogo
          // 'ambientes/isaac/isaac_mapa_azul.png',  // 1º: A sala montada em jogo
          // 'ambientes/isaac/isaac_mapa_biblioteca.png',   // 2º: A folha de tiles técnica
          'ambientes/isaac/isaac_mapa_marrom.png',    // 3º: Pedras quebrando / animação
          // 'ambientes/isaac/isaac_mapa_vermelho.png'   // 4º: Detalhes e texturas
        ]
      },
    ]
  }
])

// Lógica de Efeitos
const crtAtivo = ref(true)

// Lógica do Modal Imersivo
const imagemFocada = ref(null)
const escalaZoom = ref(1) 

const abrirModal = (caminhoImg) => {
  imagemFocada.value = caminhoImg
  escalaZoom.value = 2
  document.body.style.overflow = 'hidden'
}

const fecharModal = () => {
  imagemFocada.value = null
  escalaZoom.value = 1
  document.body.style.overflow = ''
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

function mostrarBotaoJogar(link){
  const userAgent = navigator.userAgent || navigator.vendor || window.opera;
  const isMobile = /Android|webOS|iPhone|iPad|iPod|BlackBerry|IEMobile|Opera Mini/i.test(userAgent);
  return !isMobile && (link != null || link != "");
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=VT323&family=Silkscreen&family=Chakra+Petch:wght@400;600&display=swap');

:global(body), :global(#app) {
  margin: 0; padding: 0; width: 100%; max-width: 100%;
  display: block;
}

.pixel-portfolio {
  --bg-creme: #FAF5EB;
  --bg-dots: #E8DFD3;
  --card-bg: #FBEFDE;
  --borda-marrom: #2B1817; 
  
  --sakura-light: #FFB7C5;
  --sakura-mid: #E87A90;
  --sakura-dark: #B5495B;
  --sakura-shadow: #8A3142;
  
  --verde-musgo: #4A6D55;
  --verde-borda: #213527;
  --verde-sombra: #17241A;
  
  --dourado: #D99C54;
  --wpp-green: #25D366;
  --wpp-dark: #128C7E;

  min-height: 100vh;
  padding: 5rem 1.5rem 4rem 1.5rem;
  display: flex;
  flex-direction: column;
  align-items: center;
  font-family: 'VT323', monospace;
  color: var(--borda-marrom);
  position: relative;
  
  background-color: var(--bg-creme);
  background-image: radial-gradient(var(--bg-dots) 2px, transparent 2px);
  background-size: 24px 24px;
}

/* ================== HUDS (Botões Fixos) ================== */
.btn-crt-toggle {
  position: fixed;
  top: 1.5rem;
  left: 1.5rem;
  background-color: var(--card-bg);
  color: var(--borda-marrom);
  border: 3px solid var(--borda-marrom);
  font-family: 'Silkscreen', cursive;
  font-size: 0.9rem;
  padding: 0.5rem 0.8rem;
  cursor: pointer;
  z-index: 999;
  box-shadow: 4px 4px 0px var(--sakura-mid);
  transition: all 0.1s ease;
  display: flex;
  align-items: center;
  gap: 0.5rem;
}

.btn-crt-toggle:hover {
  background-color: var(--sakura-mid);
  color: #FFF;
  transform: translate(-2px, -2px);
  box-shadow: 6px 6px 0px var(--borda-marrom);
}

.btn-crt-toggle:active {
  transform: translate(4px, 4px);
  box-shadow: 0px 0px 0px var(--borda-marrom);
}

/* Redes Sociais no Topo Direito */
.social-hud {
  position: fixed;
  top: 1.5rem;
  right: 1.6rem;
  display: flex;
  gap: 0.8rem;
  z-index: 999;
}

.social-btn {
  width: 65px;
  height: 45px;
  padding: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  font-family: 'Silkscreen', cursive;
  font-size: 0.9rem;
  border: 3px solid var(--borda-marrom);
  background-color: var(--card-bg);
  color: var(--borda-marrom);
  box-shadow: 4px 4px 0px var(--borda-marrom);
  transition: all 0.1s ease;
}

/* Estilo específico para o WhatsApp */
.social-btn.wpp {
  background-color: var(--wpp-green);
  color: #FFF;
  text-shadow: 2px 2px 0px var(--wpp-dark);
  box-shadow: 4px 4px 0px var(--wpp-dark);
  border-color: var(--wpp-dark);
}

.social-btn:hover {
  transform: translate(-2px, -2px);
}

.social-btn.wpp:hover {
  background-color: #2be871;
  box-shadow: 6px 6px 0px var(--wpp-dark);
}

.social-btn:active {
  transform: translate(4px, 4px);
  box-shadow: 0px 0px 0px transparent !important;
}

/* ================== EFEITO CRT ================== */
.crt-overlay {
  position: fixed;
  top: 0; left: 0; width: 100%; height: 100%;
  background: linear-gradient(rgba(18, 16, 16, 0) 50%, rgba(0, 0, 0, 0.03) 50%);
  background-size: 100% 4px;
  pointer-events: none;
  z-index: 900; 
}

/* ================== HERO PANEL ================== */
.hero-panel {
  background-color: var(--card-bg);
  border: 4px solid var(--borda-marrom);
  padding: 0.5rem;
  width: 100%;
  max-width: 900px;
  text-align: center;
  margin-bottom: 5rem;
  box-shadow: 10px 10px 0px var(--sakura-mid);
  transform: translateY(0);
  animation: float 4s ease-in-out infinite;
}

.hero-inner {
  border: 2px dashed var(--sakura-mid);
  padding: 3rem 2rem;
  background-color: rgba(255,255,255,0.4);
}

@keyframes float {
  0% { transform: translateY(0px); box-shadow: 10px 10px 0px var(--sakura-mid); }
  50% { transform: translateY(-8px); box-shadow: 14px 18px 0px var(--sakura-mid); }
  100% { transform: translateY(0px); box-shadow: 10px 10px 0px var(--sakura-mid); }
}

.pixel-title {
  font-family: 'Silkscreen', cursive;
  font-size: 3.5rem;
  color: var(--sakura-dark);
  margin: 0 0 0.5rem 0;
  text-shadow: 3px 3px 0px var(--sakura-light), 5px 5px 0px var(--borda-marrom);
  letter-spacing: -1px;
}

.pixel-subtitle { 
  font-family: 'Chakra Petch', sans-serif;
  font-weight: 600;
  font-size: 1.2rem; 
  color: var(--verde-musgo);
  text-transform: uppercase;
  letter-spacing: 2px;
  margin: 0 0 1.5rem 0; 
}

.divisor {
  width: 560px;
  height: 4px;
  background-color: var(--sakura-mid);
  margin: 0 auto 1.5rem auto;
}

.pixel-text { 
  font-family: 'Chakra Petch', sans-serif; 
  font-size: 1.1rem; 
  font-weight: 400; 
  line-height: 1.6; 
  color: var(--borda-marrom);
  max-width: 600px;
  margin: 0 auto;
}

/* ================== MAIN CONTENT ================== */
.conteudo-principal {
  width: 100%;
  max-width: 900px;
  display: flex;
  flex-direction: column;
  gap: 5rem;
}

.secao-bloco {
  display: flex;
  flex-direction: column;
  gap: 2rem;
}

.secao-header-wrapper {
  margin-bottom: -1rem; 
  z-index: 2;
}

.secao-header-title {
  font-family: 'Silkscreen', cursive;
  font-size: 1.6rem;
  color: #FFF;
  background-color: var(--verde-musgo);
  margin: 0;
  padding: 0.8rem 2rem;
  display: inline-block;
  border: 4px solid var(--verde-borda);
  box-shadow: 6px 6px 0px var(--sakura-mid);
  position: relative;
}

.secao-header-title::before {
  content: ''; position: absolute; top: 4px; left: 4px; right: 4px; bottom: 4px;
  border: 1px solid rgba(255,255,255,0.2);
  pointer-events: none;
}

.subsecoes-lista {
  display: flex;
  flex-direction: column;
  gap: 3rem;
}

/* ================== CARTRIDGES (CARDS) ================== */
.cartridge {
  background-color: var(--card-bg);
  border: 4px solid var(--borda-marrom);
  padding: 2rem;
  box-shadow: 8px 8px 0px var(--borda-marrom);
  position: relative;
}

.cartridge::before {
  content: ''; position: absolute; top: 0; left: 0; right: 0; height: 6px;
  background: rgba(255,255,255,0.4);
}

.cartridge-header {
  display: flex;
  justify-content: space-between;
  align-items: flex-end;
  margin-bottom: 2rem;
  padding-bottom: 1.5rem;
  border-bottom: 2px dashed var(--sakura-shadow);
  flex-wrap: wrap;
  gap: 1.5rem;
}

.cartridge-info { flex: 1; min-width: 250px; }
.pixel-title-sm { 
  font-size: 2.2rem; 
  margin: 0 0 0.5rem 0; 
  color: var(--borda-marrom); 
  text-shadow: 2px 2px 0px var(--sakura-light);
}

.pixel-text-sm { 
  font-family: 'Chakra Petch', sans-serif; 
  font-size: 1rem; 
  line-height: 1.5;
  margin: 0; 
}
.destaque-texto { font-style: italic; color: var(--verde-musgo); font-weight: 600;}

/* ================== BOTÕES GENÉRICOS ================== */
.pixel-btn {
  font-family: 'Silkscreen', cursive;
  font-size: 1.1rem;
  text-decoration: none;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: 0.5rem;
  padding: 0.8rem 1.5rem;
  cursor: pointer;
  transition: all 0.1s ease;
}

.btn-jogar {
  background-color: var(--verde-musgo);
  color: #FFF;
  border: 4px solid var(--verde-borda);
  box-shadow: 6px 6px 0px var(--sakura-mid);
}

.btn-jogar:hover { 
  background-color: #5b8769; 
  transform: translate(-2px, -2px);
  box-shadow: 8px 8px 0px var(--sakura-mid);
}

.btn-jogar:active { 
  transform: translate(6px, 6px); 
  box-shadow: 0px 0px 0px var(--sakura-mid); 
}

/* ================== GRID DE IMAGENS ================== */
.galeria-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
  gap: 1.5rem;
}

.mock-imagem {
  background-color: var(--bg-creme);
  border: 4px solid var(--borda-marrom);
  aspect-ratio: 1 / 1;
  position: relative;
  overflow: hidden; 
  cursor: pointer;
  transition: transform 0.2s cubic-bezier(0.175, 0.885, 0.32, 1.275);
  box-shadow: 4px 4px 0px rgba(0,0,0,0.15);
}

.image-overlay {
  position: absolute; top: 0; left: 0; right: 0; bottom: 0;
  background: rgba(24, 24, 24, 0.1);
  transition: opacity 0.2s;
  z-index: 1;
}

.mock-imagem:hover { 
  transform: scale(1.05) translateY(-5px); 
  box-shadow: 8px 8px 0px var(--sakura-mid);
  z-index: 2;
}
.mock-imagem:hover .image-overlay { opacity: 0; }

.pixel-art-img {
  width: 100%;
  height: 100%;
  object-fit: cover; 
  position: relative;
  z-index: 0;
  image-rendering: -moz-crisp-edges;
  image-rendering: -webkit-optimize-contrast;
  image-rendering: crisp-edges;
  image-rendering: pixelated; 
}

/* ================== MODAL LIGHTBOX ================== */
.modal-backdrop {
  position: fixed; top: 0; left: 0; right: 0; bottom: 0;
  background-color: rgba(23, 23, 23, 0.95); 
  display: flex; align-items: center; justify-content: center; 
  z-index: 1000; 
  backdrop-filter: blur(4px);
}

.fade-enter-active, .fade-leave-active { transition: opacity 0.2s ease; }
.fade-enter-from, .fade-leave-to { opacity: 0; }

.modal-image-container {
  width: 100vw; height: 100vh;
  display: flex; 
  overflow: auto; 
}

.modal-img-grande {
  margin: auto; 
  object-fit: contain; 
  transition: width 0.2s cubic-bezier(0.25, 0.46, 0.45, 0.94), height 0.2s cubic-bezier(0.25, 0.46, 0.45, 0.94); 
  image-rendering: pixelated; 
  filter: drop-shadow(0px 10px 20px rgba(0,0,0,0.5));
}

.zoom-controls {
  position: absolute; bottom: 2.5rem;
  display: flex; align-items: center; gap: 0.8rem;
  background: var(--card-bg);
  border: 4px solid var(--borda-marrom);
  padding: 0.5rem 1rem;
  box-shadow: 6px 6px 0px var(--borda-marrom);
  z-index: 1001; 
}

.divisor-vertical { width: 2px; height: 30px; background: var(--sakura-shadow); opacity: 0.3; }

.zoom-display {
  font-family: 'VT323', monospace;
  font-size: 2rem;
  color: var(--borda-marrom);
  min-width: 4rem; text-align: center;
  font-weight: bold;
}

.zoom-btn {
  font-family: 'Silkscreen', cursive;
  font-size: 1.2rem;
  background: var(--verde-musgo);
  color: #FFF;
  border: 3px solid var(--verde-borda);
  cursor: pointer;
  width: 40px; height: 40px;
  display: flex; align-items: center; justify-content: center;
  transition: all 0.1s;
  box-shadow: 3px 3px 0px var(--verde-sombra);
}

.zoom-btn:hover { background: #5b8769; transform: translate(-1px, -1px); box-shadow: 4px 4px 0px var(--verde-sombra); }
.zoom-btn:active { transform: translate(3px, 3px); box-shadow: 0px 0px 0px var(--verde-sombra); }

.btn-reset { width: auto; padding: 0 1rem; font-family: 'VT323', monospace; font-size: 1.5rem; }

.fechar-btn {
  position: absolute; top: 2rem; right: 2rem; 
  background: var(--sakura-dark); 
  color: #FFF;
  border: 4px solid var(--borda-marrom); 
  width: 55px; height: 55px; 
  font-size: 1.8rem; 
  cursor: pointer;
  display: flex; align-items: center; justify-content: center; 
  font-family: 'Silkscreen', cursive;
  z-index: 1001;
  box-shadow: 4px 4px 0px var(--borda-marrom);
  transition: all 0.1s;
}

.fechar-btn:hover { background: var(--sakura-mid); transform: translate(-2px, -2px); box-shadow: 6px 6px 0px var(--borda-marrom); }
.fechar-btn:active { transform: translate(4px, 4px); box-shadow: 0px 0px 0px var(--borda-marrom); }

::-webkit-scrollbar {
  width: 14px;
  height: 14px;
}

::-webkit-scrollbar-track {
  background: var(--bg-creme);
  border-left: 3px solid var(--borda-marrom);
}

::-webkit-scrollbar-thumb {
  background: var(--verde-musgo);
  border: 3px solid var(--borda-marrom);
  border-radius: 0; 
}

::-webkit-scrollbar-thumb:hover {
  background: var(--sakura-shadow);
}

.modal-image-container::-webkit-scrollbar-track {
  background: rgba(23, 23, 23, 0.8);
  border-left: none;
}

/* ================== RESPONSIVO (MOBILE) ================== */
@media (max-width: 768px) {
  .pixel-portfolio { padding: 4rem 1rem 2rem 1rem; }
  
  /* Ajuste dos HUDs para telas menores */
  .btn-crt-toggle { top: 0.8rem; left: 0.8rem; font-size: 0.8rem; padding: 0.4rem; }
  .social-hud { top: 0.8rem; right: 0.8rem; gap: 0.5rem; }
  .social-btn { width: 30px; height: 8px; font-size: 0.75rem; border-width: 2px;}

  .pixel-title { font-size: 2.2rem; text-shadow: 2px 2px 0px var(--sakura-light), 3px 3px 0px var(--borda-marrom); margin-top: 1rem; }
  
  .hero-panel { margin-bottom: 3rem; padding: 0.3rem; }
  .hero-inner { padding: 2rem 1rem; }
  .conteudo-principal { gap: 3rem; }

  .cartridge { padding: 1.5rem 1rem; }
  .cartridge-header { flex-direction: column; align-items: flex-start; gap: 1rem; }
  
  .galeria-grid { grid-template-columns: repeat(2, 1fr); gap: 0.8rem; }

  .pixel-btn { width: 100%; }
  
  .fechar-btn { top: 1rem; right: 1rem; width: 45px; height: 45px; font-size: 1.4rem; }
  .zoom-controls { bottom: 1.5rem; padding: 0.4rem 0.8rem; }
  .zoom-btn { width: 35px; height: 35px; font-size: 1rem; }
  .zoom-display { font-size: 1.5rem; }
  .divisor {width: 200px;}
}
</style>