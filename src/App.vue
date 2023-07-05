<template>
  <div>
    <nav>
      <div class="top-buttons">
        <router-link to="/">
          <button :class="{ active: $route.path === '/' }">Home</button>
        </router-link>
        <router-link to="/weather">
          <button :class="{ active: $route.path === '/weather' }">Cuaca</button>
        </router-link>
        <router-link to="/location">
          <button :class="{ active: $route.path === '/location' }">Lokasi</button>
        </router-link>
        <router-link to="/stopwatch">
          <button :class="{ active: $route.path === '/stopwatch' }">Stopwatch</button>
        </router-link>
        <router-link to="/tictactoe">
          <button :class="{ active: $route.path === '/tictactoe' }">Tic Tac Toe</button>
        </router-link>
        <router-link to="/photo">
          <button :class="{ active: $route.path === '/photo' }">Foto</button>
        </router-link>
      </div>
    </nav>
    <router-view></router-view>
  </div>
</template>

<script>
export default {
  name: 'App',
};
</script>

<style>
nav {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: center;
  margin-bottom: 32px;
  background-color: rgb(95, 88, 88);
}

.top-buttons {
  display: flex;
  justify-content: center;
  align-items: center;
}

nav button {
  background-color: #99627A;
  color: #fff;
  border: none;
  border-radius: 4px;
  padding: 10px 20px;
  margin: 3px;
  font-size: 16px;
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

nav button.active {
  transform: scale(1.1);
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
}

nav button:hover {
  transform: translateY(-3px);
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
}
</style>
