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
    <h1>Projects</h1>
    <div v-if="pending1">Loading...</div>
    <ul>
      <li v-for="project in projects" :key="project?._id">
      <h2>{{project?.name}}</h2>
      <h3>Project ID: {{project?._id}}</h3>
        <p>{{project?.description}}</p>
      </li>
    </ul>
  </div>
  
  <div class="form-container">
    <div id="formcreate">
    <h2>Create a New Project</h2>
    <form @submit.prevent="createProject"> 
    <label for="Name">Project Name</label>
    <input type="text" v-model="newProject.name" id="Name" placeholder="Name" required/>
  <br>
  <br>
    <label for="description">Project Description</label>
    <textarea v-model="newProject.description" id="description" placeholder="Description..." required></textarea>
  <br>
  <br>
    <button type="submit" id="createbtn" >Create Project</button>
  </form>
  </div>

  
  <div id="formdelete" >
    <h2>Delete a Project</h2>
    <form @submit.prevent="deleteProject(projectId)">
    <label for="ID">Project ID</label>
    <input type="text" v-model="projectId" id="ID" placeholder="Project Id" required/>
    <br>
    <br>
    <button type="submit" id="deletebtn">Delete Project</button>
  </form>
  </div>

  <div id="formupdate">
    <h2>Update a Project</h2>
    <form @submit.prevent="updateProject(pId)">
    <label for="Id">Project ID</label>
    <input type="text" v-model="pId" id="Id" placeholder="Project Id" required/>
    <br>
    <br>
    <label for="Name">Project Name</label>
    <input type="text" v-model="update.name" id="Name" placeholder="Name" required/>
    <br>
    <br>
    <label for="description">Project Description</label>
    <textarea v-model="update.description" id="description" placeholder="Description..." required></textarea>
    <br>
    <br>
    <button type="submit" id="updatebtn">Update Project</button>
  </form>
  </div>
  </div>
  
  <div>
    <h1>Skill</h1>
    <div v-if="pending2">Loading...</div>
    <ul>
      <li v-for="skill in skill" :key="skill._id">
        <h2>{{skill.name}}</h2>
        <p>{{skill.description}}</p>
      </li>
    </ul>
  </div>
  
</template>

<script setup>
const name = "Navoda Rajapakshe" ;

const {data: projects, pending1, error1} = useFetch('http://localhost:5000/projects'); //usefetch calls for data from projects
const {data: skill, pending2, error2 } = useFetch('http://localhost:5000/skill');

const newProject = ref({
  name: '',
  description: ''
});
//create new project
const createProject = async () => {
  try{
  const response = await fetch('http://localhost:5000/projects', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json'
    },
    body: JSON.stringify(newProject.value)
  });
  if (!response.ok) {
      throw new Error('Failed to create project');
    }
    //add the new project to the list
    const project = await response.json();
    projects.value.push(project);
    //clear the input field
    newProject.value.name = '';
    newProject.value.description = '';
  return true;
  }catch (error) {
    console.error('Error creating project:', error.message);
    return false;
  }
  } 

//delete project
const projectId = ref('');
const deleteProject = async (_id) => {
  try{
    const response = await fetch(`http://localhost:5000/projects/${_id}`, {
      method: 'DELETE'
    });
    if (!response.ok) {
      throw new Error('Failed to delete project');
    }
    //remove the project from the list
    const index = projects.value.findIndex(project => project._id === _id);
    if (index !== -1) {
      projects.value.splice(index, 1);
    }
    //clear the input field
    projectId.value = '';
  } catch (error) {
    console.error('Error deleting project:', error.message);
  }
}

//update project by id
const pId = ref('');
const update = ref({
  name: '',
  description: ''
});
const updateProject = async (_id) => {
  try{
    const response = await fetch(`http://localhost:5000/projects/${_id}`, {
      method: 'PATCH',
      headers: {
        'Content-Type': 'application/json'
      },
      body: JSON.stringify(update.value)
    });
    if (!response.ok) {
      throw new Error('Failed to update project');
    }
    //update the project in the list
    const project = projects.value.find(project => project._id === _id);
    if (project) {
      project.name = update.value.name;
      project.description = update.value.description;
    }
    //clear the input field
    pId.value = '';
    update.value.name = '';
    update.value.description = '';
    return true;
  } catch (error) {
    console.error('Error updating project:', error.message);
    return false;
  }
}
</script>


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
.form-container {
  display: flex;
  flex-wrap: wrap;
  gap: 20px; 
  padding: 20px;
  justify-content: space-between; 
}

.form-container > div {
  flex: 1; 
  min-width: 300px; 
  padding: 20px;
  border: 1px solid #ccc;
  border-radius: 5px;
  background-color: #f9f9f9;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  transition: transform 0.2s; 
}

.form-container > div:hover {
  transform: scale(1.02); 
}

h2 {
  font-family: 'Arial', sans-serif;
  color: #333;
}

label {
  font-family: 'Arial', sans-serif;
  color: #555;
  font-weight: bold;
  display: block;
  margin-bottom: 8px;
}

input[type="text"],
textarea {
  width: 100%;
  padding: 10px;
  margin-bottom: 20px;
  border: 1px solid #ccc;
  border-radius: 4px;
  box-sizing: border-box;
  font-family: 'Arial', sans-serif;
}

textarea {
  resize: vertical;
}
button {
  border: none;
  color: white;
  padding: 8px 16px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 12px;
  margin: 4px 2px;
  transition-duration: 0.4s;
  cursor: pointer;
}
#createbtn {
  background-color: white; 
  color: black; 
  border: 2px solid #04AA6D;
}
#createbtn:hover {
  background-color: #04AA6D;
  color: white;
}
#deletebtn {
  background-color: white; 
  color: black; 
  border: 2px solid #f44336;
}
#deletebtn:hover {
  background-color: #f44336;
  color: white;
}
#updatebtn {
  background-color: white; 
  color: black; 
  border: 2px solid #008CBA;
}
#updatebtn:hover {
  background-color: #008CBA;
  color: white;
}
</style>





