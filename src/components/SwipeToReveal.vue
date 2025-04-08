<template>
  <div class="swipe-reveal">
    <h2>Make your choice</h2>
    <p>Swipe left or right to reveal your options</p>
    
    <div class="swipe-container">
      <div class="decision-bars">
        <div class="decision left" :class="{ active: swipeDirection === 'left' }">
          Option A
        </div>
        <div class="decision right" :class="{ active: swipeDirection === 'right' }">
          Option B
        </div>
      </div>
      <br>
      <div 
      
        class="card-stack"
        @touchstart="startDrag"
        @touchmove="dragCard"
        @touchend="endDrag"
        @mousedown="startDrag"
        @mousemove="dragCard"
        @mouseup="endDrag"
      >
        <div 
          v-for="(card, index) in cards" 
          :key="index"
          class="swipe-card"
          :class="{
            'current-card': index === currentCardIndex,
            'swiping-left': isSwiping && swipeDirection === 'left',
            'swiping-right': isSwiping && swipeDirection === 'right'
          }"
          :style="{
            transform: index === currentCardIndex ? 
              `translateX(${dragOffset}px) rotate(${rotation}deg)` : '',
            zIndex: cards.length - index,
            background: card.color
          }"
          v-show="index >= currentCardIndex"
        >
          <div class="card-content">
            <div class="icon">{{ card.icon }}</div>
            <h3>{{ card.text }}</h3>
          </div>
        </div>
      </div>

      <div class="action-buttons">
        <button class="reject" @click="swipeLeft">
          <span class="action-icon">👈</span>
        </button>
        <button class="accept" @click="swipeRight">
          <span class="action-icon">👉</span>
        </button>
      </div>
    </div>

    <div class="feedback" :class="feedbackClass" v-if="feedback">
      {{ feedback }}
    </div>
  </div>
</template>

<script>
export default {
  name: 'SwipeToReveal',
  data() {
    return {
      cards: [
        { text: 'Choose this option', icon: '🤔', color: 'linear-gradient(135deg, #FF9FF3 0%, #F368E0 100%)' },
        { text: 'Select this instead', icon: '🤩', color: 'linear-gradient(135deg, #FECA57 0%, #FF9F43 100%)' },
        { text: 'Pick your favorite', icon: '😍', color: 'linear-gradient(135deg, #1DD1A1 0%, #10AC84 100%)' },
        { text: 'Decide now', icon: '🤗', color: 'linear-gradient(135deg, #54A0FF 0%, #2E86DE 100%)' }
      ],
      currentCardIndex: 0,
      dragStartX: 0,
      dragOffset: 0,
      isSwiping: false,
      swipeDirection: null,
      feedback: '',
      feedbackClass: ''
    }
  },
  computed: {
    rotation() {
      const maxRotation = 15;
      const rotation = (this.dragOffset / window.innerWidth) * maxRotation * 2;
      return Math.min(maxRotation, Math.max(-maxRotation, rotation));
    }
  },
  methods: {
    startDrag(e) {
      this.isSwiping = true;
      this.dragStartX = e.type.includes('touch') ? e.touches[0].clientX : e.clientX;
    },
    dragCard(e) {
      if (!this.isSwiping) return;
      
      const x = e.type.includes('touch') ? e.touches[0].clientX : e.clientX;
      this.dragOffset = x - this.dragStartX;
      
      if (this.dragOffset > 0) {
        this.swipeDirection = 'right';
      } else if (this.dragOffset < 0) {
        this.swipeDirection = 'left';
      } else {
        this.swipeDirection = null;
      }
    },
    endDrag() {
      if (!this.isSwiping) return;
      this.isSwiping = false;
      
      const threshold = window.innerWidth / 4;
      if (Math.abs(this.dragOffset) > threshold) {
        if (this.dragOffset > 0) {
          this.swipeRight();
        } else {
          this.swipeLeft();
        }
      } else {
        this.resetCardPosition();
      }
    },
    swipeLeft() {
      this.showFeedback('You chose Option A!', 'success');
      this.animateCardSwipe('left');
    },
    swipeRight() {
      this.showFeedback('You chose Option B!', 'success');
      this.animateCardSwipe('right');
    },
    showFeedback(message, type) {
      this.feedback = message;
      this.feedbackClass = type;
      
      setTimeout(() => {
        this.feedback = '';
      }, 1500);
    },
    animateCardSwipe(direction) {
      const cardWidth = window.innerWidth;
      this.dragOffset = direction === 'right' ? cardWidth : -cardWidth;
      
      setTimeout(() => {
        this.currentCardIndex++;
        this.resetCardPosition();
      }, 300);
    },
    resetCardPosition() {
      this.dragOffset = 0;
      this.swipeDirection = null;
    }
  }
}
</script>

<style scoped>
.swipe-reveal {
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

.swipe-container {
  width: 100%;
  max-width: 400px;
  height: 450px;
  margin: 0 auto;
  position: relative;
}

.decision-bars {
  display: flex;
  justify-content: space-between;
  position: absolute;
  top: 20px;
  left: 0;
  right: 0;
  z-index: 10;
  padding: 0 20px;
  pointer-events: none;
}

.decision {
  padding: 10px 20px;
  border-radius: 20px;
  font-weight: bold;
  font-size: 14px;
  color: transparent;
  border: 3px solid transparent;
  transition: all 0.3s ease;
}

.decision.left {
  border-color: var(--primary);
}

.decision.right {
  border-color: var(--accent);
}

.decision.active {
  color: white;
}

.decision.left.active {
  background: var(--primary);
}

.decision.right.active {
  background: var(--accent);
}

.card-stack {
  width: 100%;
  height: 100%;
  position: relative;
  display: flex;
  justify-content: center;
  align-items: center;
}

.swipe-card {
  width: 85%;
  height: 70%;
  border-radius: 20px;
  position: absolute;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  box-shadow: 0 10px 30px rgba(0,0,0,0.1);
  transition: transform 0.3s ease;
  user-select: none;
  touch-action: none;
}

.current-card {
  cursor: grab;
}

.swiping-left {
  background: var(--primary) !important;
}

.swiping-right {
  background: var(--accent) !important;
}

.card-content {
  text-align: center;
  padding: 20px;
  color: white;
}

.card-content h3 {
  font-size: 1.5rem;
  margin-top: 20px;
  text-shadow: 0 2px 4px rgba(0,0,0,0.2);
}

.icon {
  font-size: 3.5rem;
}

.action-buttons {
  display: flex;
  justify-content: center;
  gap: 40px;
  margin-top: 30px;
}

.action-buttons button {
  width: 70px;
  height: 70px;
  border-radius: 50%;
  border: none;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  background: white;
  box-shadow: 0 5px 15px rgba(0,0,0,0.1);
  transition: transform 0.2s ease;
}

.action-buttons button:active {
  transform: scale(0.95);
}

.action-icon {
  font-size: 1.8rem;
}

.reject {
  color: var(--primary);
  border: 2px solid var(--primary);
}

.accept {
  color: var(--accent);
  border: 2px solid var(--accent);
}

.feedback {
  position: fixed;
  bottom: 100px;
  left: 50%;
  transform: translateX(-50%);
  padding: 15px 30px;
  border-radius: 30px;
  font-size: 1.2rem;
  font-weight: bold;
  opacity: 0;
  animation: fadeInOut 2s ease;
  color: white;
}

.feedback.success {
  background: var(--success);
}

@keyframes fadeInOut {
  0% { opacity: 0; transform: translateX(-50%) translateY(20px); }
  20% { opacity: 1; transform: translateX(-50%) translateY(0); }
  80% { opacity: 1; transform: translateX(-50%) translateY(0); }
  100% { opacity: 0; transform: translateX(-50%) translateY(-20px); }
}

@media (max-width: 500px) {
  .swipe-container {
    height: 400px;
  }
  
  .card-content h3 {
    font-size: 1.3rem;
  }
  
  .action-buttons {
    gap: 30px;
  }
  
  .action-buttons button {
    width: 60px;
    height: 60px;
  }
}
</style>