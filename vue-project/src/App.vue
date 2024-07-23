<!-- <script setup>
// import HelloWorld from './components/HelloWorld.vue'
// import TheWelcome from './components/TheWelcome.vue'
import UserActivity from './components/UserActivity.vue';
import UserDashboard from './components/UserDashboard.vue';
</script> -->

<template>
  <div id="app">
    <h1>User Orbits</h1>
    <div v-if="loading">Loading...</div>
    <div v-else-if="error">Error: {{ error }}</div>
    <div v-else class="orbit-container" @wheel="onScroll">
      <!-- ... existing orbit content ... -->
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      orbits: [],
      startDate: '2024-1-8',
      loading: true,
      error: null
    };
  },
  methods: {
    fetchOrbits() {
      this.loading = true;
      this.error = null;
      axios.post('https://xsrr-l2ye-dpbj.f2.xano.io/api:oUvfVMO5', {
        start_date: this.startDate
      })
      .then(response => {
        this.orbits = response.data.dates.map(date => ({
          date,
          orbs: response.data.users.map(user => ({
            image: user.avatar || 'https://via.placeholder.com/50'
          }))
        }));
        this.loading = false;
      })
      .catch(error => {
        console.error('Error fetching orbits:', error);
        this.error = 'Failed to load orbits. Please try again.';
      this.loading = false;
        this.orbits = [
          {
            date: '2024-01-08',
            orbs: [
              { image: 'https://via.placeholder.com/50?text=User1' },
              { image: 'https://via.placeholder.com/50?text=User2' }
            ]
          }
        ];
      });
    },
    onScroll(event) {
      if (event.deltaY > 0) {
        this.scrollDown();
      } else {
        this.scrollUp();
      }
    },
    scrollDown() {
      console.log('Scrolling down');
    },
    scrollUp() {
      console.log('Scrolling up');
    }
  },
  mounted() {
    this.fetchOrbits();
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.orbit-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  overflow: hidden;
  height: 100vh;
}

.orbit {
  display: flex;
  justify-content: center;
  align-items: center;
  position: relative;
  border: 1px solid rgba(255, 255, 255, 0.2);
  border-radius: 50%;
  width: 80%;
  height: 80%;
  margin: 20px 0;
  background-color: rgba(200, 200, 200, 0.1);
  min-height: 300px;
}

.orb {
  position: absolute;
  display: flex;
  justify-content: center;
  align-items: center;
}

.orb img {
  border-radius: 50%;
  width: 50px;
  height: 50px;
}
</style>
