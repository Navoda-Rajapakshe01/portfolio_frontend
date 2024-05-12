<template>
  <div class="navbar">
    <ul class="navul">
      <li ><a href="#contact">Contact</a></li>
      <li ><a href="#resume">Resume</a></li>
      <li ><a href="#project">Projects</a></li>
      <li ><a href="#about">About</a></li>
     <li ><a class="active" href="#home">Home</a></li>
</ul>
  </div>

  <div>
    <h1>{{ name }}</h1>
    <h2>Projects</h2>
    <div v-if="pending1">Loading...</div>
    <ul>
      <li v-for="project in projects" :key="project.name">
      <h2>{{project.name}}</h2>
        <p>{{project.description}}</p>
      </li>
    </ul>
  </div>
  <div>
    <h2>Skill</h2>
    <div v-if="pending2">Loading...</div>
    <ul>
      <li v-for="skill in skill" :key="skill.name">
        <h2>{{skill.name}}</h2>
        <p>{{skill.description}}</p>
      </li>
    </ul>
  </div>
</template>
<style>


.navbar{
  margin: 30px;
}

.navbar ul {
  list-style-type: none;
  font-family: sans-serif;
  margin: 20;
  padding: 30;
  overflow: hidden;
}

.navbar li {
  float: right;
}

.navbar li a {
  display: block;
  color: black;
  text-align: center;
  padding: 14px 16px;
  text-decoration: none;
}
.navbar li a .active{
  color: #007EA7;
}

</style>

<script setup>
 const name = "Navoda Rajapakshe" ;
const {data: projects, pending1, error1} = useFetch('http://localhost:5000/projects');
const {data: skill, pending2, error2 } = useFetch('http://localhost:5000/skill');
</script>



