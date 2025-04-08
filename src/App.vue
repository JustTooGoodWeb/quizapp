<template>
  <div class="app">
    <div class="header">
      <h1>Reveal Game</h1>
      <div class="nav-tabs">
        <button 
          @click="currentView = 'tap'" 
          :class="{ active: currentView === 'tap' }"
        >
          Tap to Reveal
        </button>
        <button 
          @click="currentView = 'swipe'" 
          :class="{ active: currentView === 'swipe' }"
        >
          Swipe to Reveal
        </button>
      </div>
    </div>
    
    <div class="content">
      <TapToReveal v-if="currentView === 'tap'" />
      <SwipeToReveal v-if="currentView === 'swipe'" />
    </div>
  </div>
</template>

<script>
import TapToReveal from './components/TapToReveal.vue'
import SwipeToReveal from './components/SwipeToReveal.vue'

export default {
  name: 'App',
  components: {
    TapToReveal,
    SwipeToReveal
  },
  data() {
    return {
      currentView: 'tap'
    }
  }
}
</script>

<style>
/* Base Styles */
:root {
  --primary: #6C5CE7;
  --secondary: #A29BFE;
  --accent: #FD79A8;
  --light: #F8F9FA;
  --dark: #2D3436;
  --success: #00B894;
  --warning: #FDCB6E;
  --danger: #E17055;
}

* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  background-color: #f5f5f5;
  color: var(--dark);
}

.app {
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
}

.header {
  text-align: center;
  margin-bottom: 30px;
}

h1 {
  color: var(--primary);
  margin-bottom: 20px;
  font-size: 2.5rem;
  font-weight: 700;
}

.nav-tabs {
  display: flex;
  justify-content: center;
  gap: 15px;
  margin-bottom: 20px;
}

.nav-tabs button {
  padding: 12px 25px;
  border: none;
  border-radius: 30px;
  background-color: var(--light);
  color: var(--dark);
  font-size: 1rem;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s ease;
  box-shadow: 0 2px 5px rgba(0,0,0,0.1);
}

.nav-tabs button.active {
  background-color: var(--primary);
  color: white;
  box-shadow: 0 4px 8px rgba(108, 92, 231, 0.3);
}

.content {
  background-color: white;
  border-radius: 20px;
  padding: 25px;
  box-shadow: 0 5px 15px rgba(0,0,0,0.05);
}

/* Responsive */
@media (max-width: 600px) {
  .app {
    padding: 15px;
  }
  
  h1 {
    font-size: 2rem;
  }
  
  .nav-tabs {
    flex-direction: column;
    gap: 10px;
  }
  
  .nav-tabs button {
    width: 100%;
  }
}
</style>