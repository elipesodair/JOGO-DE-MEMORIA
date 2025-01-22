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
.card-container {
  display: grid; 
  grid-template-columns: repeat(5, 1fr); 
  gap: 20px; 
  padding: 40px; 
  max-width: 1400px; 
  margin: 0 auto; 
}


.card {
  width: 100%; 
  aspect-ratio: 2 / 3;
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


@media (max-width: 1024px) {
  .card-container {
    grid-template-columns: repeat(4, 1fr); 
  }
}

@media (max-width: 768px) {
  .card-container {
    grid-template-columns: repeat(3, 1fr); 
  }
}

@media (max-width: 480px) {
  .card-container {
    grid-template-columns: repeat(2, 1fr); 
  }
}
</style>


