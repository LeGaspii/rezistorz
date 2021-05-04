<template>
  <div id="app">
    <div id="nav" class="navbar navbar-expand-lg navbar-light bg-light">
          <a class="navbar-brand" href="#">Standard Resistor Value</a>
    <div class="" id="navbarNav">
        <ul class="navbar-nav">
        <li class="nav-item active">
            <a class="nav-link border rounded p-1 bg-secondary text-white" href="/">Refresh</a>
        </li>
        <!-- <li class="nav-item">
            <a class="nav-link" href="#">Pricing</a>
        </li>
        <li class="nav-item">
            <a class="nav-link disabled" href="#">Disabled</a>
        </li> -->
        </ul>
    </div>
    </div>
    <router-view/>
  </div>
</template>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

#nav {
  /* padding: 30px; */
  height-max: 100px;
}

#nav a {
  font-weight: bold;
  color: #2c3e50;
}

</style>
