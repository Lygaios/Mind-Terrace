<template>
  <div class="home">
    <div class="hero-section">
      <h2>Welcome to Mind Terrace</h2>
      <p>Your peaceful space for focused productivity</p>
    </div>
    
    <div class="main-content">
      <div class="left-panel">
        <TodoList />
      </div>
      
      <div class="right-panel">
        <PomodoroTimer />
      </div>
    </div>
  </div>
</template>

<script>
import TodoList from '../components/TodoList.vue'
import PomodoroTimer from '../components/PomodoroTimer.vue'

export default {
  name: 'Home',
  components: {
    TodoList,
    PomodoroTimer
  }
}
</script>

<style scoped>
.home {
  max-width: 1200px;
  margin: 0 auto;
}

.hero-section {
  text-align: center;
  margin-bottom: 3rem;
  padding: 2rem;
  background: rgba(255, 255, 255, 0.1);
  backdrop-filter: blur(10px);
  border-radius: 20px;
  border: 1px solid rgba(255, 255, 255, 0.2);
}

.hero-section h2 {
  color: #2c3e50;
  font-size: 2.5rem;
  margin-bottom: 1rem;
  font-weight: 300;
}

.hero-section p {
  color: #34495e;
  font-size: 1.2rem;
  opacity: 0.8;
}

.main-content {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 2rem;
}

.left-panel,
.right-panel {
  background: rgba(255, 255, 255, 0.1);
  backdrop-filter: blur(10px);
  border-radius: 20px;
  border: 1px solid rgba(255, 255, 255, 0.2);
  padding: 2rem;
  min-height: 500px;
}

@media (max-width: 768px) {
  .main-content {
    grid-template-columns: 1fr;
  }
}
</style>