<template>
  <div class="navbar">
    <ul>
      <li><a href="#contact">Contact</a></li>
      <li><a href="#resume">Resume</a></li>
      <li><a href="#project">Projects</a></li>
      <li><a href="#about">About</a></li>
     <li><a class="active" href="#home">Home</a></li>
</ul>
  </div>
  <div class="container">
  <div class="description">
    <h1>NAVODA RAJAPAKSHE</h1>
    <p>A first year ITM undergraduate at Faculty of Information Technology, University of Moratuwa, Sri Lanka. .</p>
  </div>
  <div class="image">
    <img src="C:\Users\acer\Desktop\portfolio\portfolio_nuxt\portfolio_frontend\portfolio\homeimage.webp" alt="Image">
  </div>
</div>
</template>
<style>

body {
  background-color: #000000; 
  background-image: url("C:\Users\acer\Desktop\portfolio\portfolio_nuxt\portfolio_frontend\portfolio\homebackground.jpg"); 
  background-size: cover;
}
.navbar{
  margin: 30px;
}

ul {
  list-style-type: none;
  font-family: sans-serif;
  margin: 20;
  padding: 30;
  overflow: hidden;
}

li {
  float: right;
}

li a {
  display: block;
  color: white;
  text-align: center;
  padding: 14px 16px;
  text-decoration: none;
}
.active{
  color: #007EA7;
}
h1{
  color: white;
  font-family: sans-serif;
  font-size: 50px;
}
.container {
    display: flex; 
    align-items: center; 
    justify-content: space-between; 
    max-width: 1020px; 
    margin: 0 auto; 
    padding: 20px 10px;
  }

  .description {
    flex: 1;
    margin-right: 40px; 
    color: white;
    font-size: 20px;
    font-family: sans-serif;
  }

  .image {
    flex: 1; 
    
  }

  img {
    max-width: 100%; 
    height: auto;
    margin: 40px;
    border-radius: 30px;
  }
</style>

<script>
export default {
  css: [
    '~/assets/global.css'
  ]
}

</script>



