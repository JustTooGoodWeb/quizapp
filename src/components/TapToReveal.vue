<template>
  <div class="tap-reveal">
    <h2>What's hidden?</h2>
    <p>Tap the cards to reveal what's behind them</p>
    
    <div class="card-grid">
      <div 
        v-for="(card, index) in cards" 
        :key="index" 
        class="card" 
        :class="{ flipped: card.flipped }"
        @click="flipCard(index)"
        :style="`--card-color: ${card.color}`"
      >
        <div class="card-inner">
          <div class="card-front">
            <div class="icon-container">
              <span class="icon">👀</span>
            </div>
          </div>
          <div class="card-back">
            <div class="icon-container">
              <span class="icon">{{ card.icon }}</span>
            </div>
            <h3>{{ card.title }}</h3>
            <p>{{ card.description }}</p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'TapToReveal',
  data() {
    return {
      cards: [
        { 
          title: 'Surprise!', 
          description: 'You found a hidden treasure!', 
          icon: '🎁',
          color: '#FF9FF3',
          flipped: false
        },
        { 
          title: 'Congratulations', 
          description: 'You unlocked a secret message!', 
          icon: '🔓',
          color: '#FECA57',
          flipped: false
        },
        { 
          title: 'Bonus', 
          description: 'You earned extra points!', 
          icon: '⭐',
          color: '#1DD1A1',
          flipped: false
        },
        { 
          title: 'Special', 
          description: 'You discovered something unique!', 
          icon: '🌈',
          color: '#54A0FF',
          flipped: false
        }
      ]
    }
  },
  methods: {
    flipCard(index) {
      this.cards[index].flipped = !this.cards[index].flipped;
    }
  }
}
</script>

<style scoped>
.tap-reveal {
  text-align: center;
}

h2 {
  color: var(--primary);
  margin-bottom: 10px;
  font-size: 1.8rem;
}

p {
  color: var(--dark);
  opacity: 0.8;
  margin-bottom: 30px;
  font-size: 1.1rem;
}

.card-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 20px;
}

.card {
  background: var(--card-color);
  border-radius: 16px;
  height: 200px;
  perspective: 1000px;
  cursor: pointer;
  transition: transform 0.3s, box-shadow 0.3s;
  transform-style: preserve-3d;
  box-shadow: 0 5px 15px rgba(0,0,0,0.1);
}

.card:hover {
  transform: translateY(-5px);
  box-shadow: 0 8px 20px rgba(0,0,0,0.15);
}

.card.flipped {
  transform: rotateY(180deg);
}

.card-inner {
  position: relative;
  width: 100%;
  height: 100%;
  transition: transform 0.6s;
  transform-style: preserve-3d;
}

.card-front, .card-back {
  position: absolute;
  width: 100%;
  height: 100%;
  backface-visibility: hidden;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  border-radius: 16px;
  padding: 20px;
  box-sizing: border-box;
}

.card-front {
  background: var(--card-color);
  color: white;
}

.card-back {
  background: white;
  color: var(--dark);
  transform: rotateY(180deg);
  text-align: center;
}

.icon-container {
  width: 70px;
  height: 70px;
  background: rgba(255,255,255,0.3);
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  margin-bottom: 15px;
}

.icon {
  font-size: 2.5rem;
}

.card-back h3 {
  margin: 10px 0 5px;
  color: var(--primary);
  font-size: 1.3rem;
}

.card-back p {
  color: #666;
  font-size: 0.95rem;
  line-height: 1.4;
  margin: 0;
  opacity: 1;
}

@media (max-width: 500px) {
  .card-grid {
    grid-template-columns: 1fr;
  }
  
  .card {
    height: 180px;
  }
}
</style>