<template>
  <div id="game">
    <div v-if="!gameStarted" class="start-screen">
      <h1>Bem-vindo ao Jogo da Memória!</h1>
      <input v-model="playerName" placeholder="Digite seu nome" />
      <button @click="startGame">Iniciar Jogo</button>
    </div>

    <div v-else class="game-screen">
      <h2>Jogador: {{ playerName }}</h2>
      <p>Rodadas: {{ rounds }}</p>
      <div class="grid">
        <Card
          v-for="(card, index) in cards"
          :key="index"
          :card="card"
          :index="index"
          @flip-card="flipCard"
        />
      </div>
      <div v-if="gameWon" class="win-message">
        <h2>Parabéns, {{ playerName }}! Você venceu o jogo!</h2>
        <p>Rodadas utilizadas: {{ rounds }}</p>
        <button @click="restartGame">Jogar Novamente</button>
      </div>
    </div>
  </div>
</template>

<script>
import Card from "@/components/MemoryCard.vue";

export default {
  components: { Card },
  data() {
    return {
      playerName: "",
      gameStarted: false,
      cards: [],
      flippedCards: [],
      rounds: 0,
      gameWon: false,
      values: ["🍎", "🍌", "🍒", "🍇", "🍓", "🍍", "🥝", "🍉", "🍑", "🍋"],
    };
  },
  methods: {
    startGame() {
      if (this.playerName.trim() === "") {
        alert("Por favor, insira seu nome.");
        return;
      }
      this.initializeCards();
      this.gameStarted = true;
    },
    initializeCards() {
      const duplicatedValues = [...this.values, ...this.values];
      this.cards = duplicatedValues
        .sort(() => Math.random() - 0.5)
        .map((value) => ({ value, flipped: false, matched: false }));
      this.rounds = 0;
      this.gameWon = false;
    },
    flipCard(index) {
      const card = this.cards[index];
      if (card.flipped || card.matched || this.flippedCards.length === 2) {
        return;
      }
      card.flipped = true;
      this.flippedCards.push(index);

      if (this.flippedCards.length === 2) {
        this.rounds++;
        this.checkMatch();
      }
    },
    checkMatch() {
      const [firstIndex, secondIndex] = this.flippedCards;
      const firstCard = this.cards[firstIndex];
      const secondCard = this.cards[secondIndex];

      if (firstCard.value === secondCard.value) {
        firstCard.matched = true;
        secondCard.matched = true;
        this.checkWin();
      } else {
        setTimeout(() => {
          firstCard.flipped = false;
          secondCard.flipped = false;
        }, 1000);
      }
      this.flippedCards = [];
    },
    checkWin() {
      if (this.cards.every((card) => card.matched)) {
        this.gameWon = true;
      }
    },
    restartGame() {
      this.playerName = "";
      this.gameStarted = false;
      this.cards = [];
      this.flippedCards = [];
      this.rounds = 0;
      this.gameWon = false;
    },
  },
};
</script>


<style scoped>
/* Estilo global */
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  background-color: #f0f4f8;
  overflow: hidden;
}

.start-screen {
  text-align: center;
  background-color: #f4f7fc;
  padding: 50px 20px;
  border-radius: 10px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
  max-width: 500px;
  width: 90%;
}

.start-screen h1 {
  font-size: 2.5rem;
  color: #333;
  margin-bottom: 20px;
  font-weight: bold;
}

.start-screen input {
  width: 100%;
  padding: 10px;
  margin: 10px 0;
  border: 2px solid #ddd;
  border-radius: 5px;
  font-size: 1rem;
  transition: all 0.3s ease;
}

.start-screen input:focus {
  border-color: #4CAF50;
  outline: none;
  box-shadow: 0 0 5px rgba(76, 175, 80, 0.7);
}

.start-screen button {
  padding: 12px 30px;
  background-color: #4CAF50;
  color: white;
  border: none;
  border-radius: 5px;
  font-size: 1.1rem;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.start-screen button:hover {
  background-color: #45a049;
}

.start-screen, .game-screen {
  max-width: 100%; /* Garante que a largura não ultrapasse o limite */
  width: 100%; /* Ajusta à largura da tela */
  padding: 20px;
  box-sizing: border-box; /* Inclui padding na largura total */
  text-align: center;
}

/* Tela do jogo */
.game-screen {
  text-align: center;
  background-color: #fff;
  padding: 40px 20px;
  border-radius: 10px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
  max-width: 1000px;
  width: 95%;
}

.game-screen h2 {
  font-size: 2rem;
  color: #333;
  margin-bottom: 10px;
  font-weight: bold;
}

.game-screen p {
  font-size: 1.2rem;
  color: #666;
  margin-bottom: 20px;
}

/* Grade de cartas */
.grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(70px, 1fr)); /* Ajusta dinamicamente o número de colunas */
  gap: 16px; /* Espaçamento consistente */
  justify-content: center;
  align-items: center;
  margin: 0 auto;
  padding: 10px;
  width: 100%;
  max-width: 1200px; /* Limita a largura máxima da grade */
}

.card {
  width: 100%; /* Ajusta dinamicamente com base no grid */
  aspect-ratio: 3 / 4; /* Mantém a proporção */
  border: 1px solid #ccc;
  border-radius: 5px;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  position: relative;
  perspective: 1000px;
  background-color: #fff;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  transition: transform 0.3s ease;
}

.card:hover {
  transform: scale(1.05);
}

.card .front,
.card .back {
  position: absolute;
  width: 100%;
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  backface-visibility: hidden;
  transition: transform 0.3s;
}

.card .front {
  background: #f9f9f9;
  transform: rotateY(0deg);
}

.card .back {
  background: #4caf50;
  color: white;
  transform: rotateY(180deg);
  font-size: 3rem;
}

.card .back img {
  width: 80%; /* Aumenta o tamanho da imagem */
  height: auto;
  object-fit: contain;
}

.card.flipped .front {
  transform: rotateY(180deg);
}

.card.flipped .back {
  transform: rotateY(0deg);
}

/* Mensagem de vitória */
.win-message {
  background-color: #f4f7fc;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
  margin-top: 30px;
}

.win-message h2 {
  font-size: 2rem;
  color: #4CAF50;
  font-weight: bold;
}

.win-message p {
  font-size: 1.2rem;
  color: #333;
  margin-bottom: 20px;
}

.win-message button {
  padding: 12px 30px;
  background-color: #FF5722;
  color: white;
  border: none;
  border-radius: 5px;
  font-size: 1.1rem;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.win-message button:hover {
  background-color: #e64a19;
}

/* Media Queries */
@media (max-width: 1024px) {
  .grid {
    gap: 12px; /* Reduz o espaçamento em telas menores */
  }

  .card {
    aspect-ratio: 2.5 / 3; /* Ajusta a proporção para telas menores */
  }
}

@media (max-width: 768px) {
  .grid {
    grid-template-columns: repeat(auto-fit, minmax(80px, 1fr)); /* Ajusta o número de colunas */
  }

  .card {
    aspect-ratio: 2 / 3; /* Mantém proporções consistentes */
  }
}

@media (max-width: 480px) {
  .grid {
    gap: 8px; /* Reduz o espaçamento ainda mais */
  }

  .card {
    aspect-ratio: 1.8 / 3; /* Proporções mais estreitas para telas pequenas */
  }
}

@keyframes wrongEffect {
  0% {
    transform: scale(1);
    background-color: #f44336;
  }
  50% {
    transform: scale(0.9);
    background-color: #e57373;
  }
  100% {
    transform: scale(1);
    background-color: #f44336;
  }
}
</style>



