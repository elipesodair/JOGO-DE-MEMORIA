<template>
  <div
    class="card"
    :class="{ flipped: card.flipped || card.matched }"
    @click="flipCard"
  >
    <div class="front">?</div>
    <div class="back">{{ card.value }}</div>
  </div>
</template>

<script>
export default {
  props: {
    card: {
      type: Object,
      required: true,
    },
    index: {
      type: Number,
      required: true,
    },
  },
  methods: {
    flipCard() {
      this.$emit("flip-card", this.index);
    },
  },
};
</script>

<style scoped>
/* Container dos cards */
.card-container {
  display: grid; /* Troquei para Grid Layout */
  grid-template-columns: repeat(5, 1fr); /* Define 5 colunas de tamanho igual */
  gap: 20px; /* Espaçamento entre os cards */
  padding: 40px; /* Espaçamento interno no container */
  max-width: 1400px; /* Largura máxima do container */
  margin: 0 auto; /* Centraliza o container na página */
}

/* Estilo dos cards */
.card {
  width: 100%; /* Ajusta automaticamente o tamanho com base na coluna */
  aspect-ratio: 2 / 3; /* Mantém a proporção de 2:3 (ex.: 120px x 180px) */
  border: 1px solid #ccc;
  border-radius: 5px;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  position: relative;
  perspective: 1000px;
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
}

.card.flipped .front {
  transform: rotateY(180deg);
}

.card.flipped .back {
  transform: rotateY(0deg);
}

/* Responsividade */
@media (max-width: 1024px) {
  .card-container {
    grid-template-columns: repeat(4, 1fr); /* 4 colunas em telas médias */
  }
}

@media (max-width: 768px) {
  .card-container {
    grid-template-columns: repeat(3, 1fr); /* 3 colunas em telas menores */
  }
}

@media (max-width: 480px) {
  .card-container {
    grid-template-columns: repeat(2, 1fr); /* 2 colunas em telas pequenas */
  }
}
</style>


