<template>
  <div class="navbar">
    <ul class="navul">
      <li><a href="#contact">Contact</a></li>
      <li><a href="#resume">Resume</a></li>
      <li><a href="#project">Projects</a></li>
      <li><a href="#about">About</a></li>
      <li><a class="active" href="#home">Home</a></li>
    </ul>
  </div>
  
  <h1>{{ name }}</h1>

  <div class="project-container">
    <h1>Projects</h1>
    <div class="project-list">
    <div v-if="pending1">Loading...</div>
    <ul>
      <li v-for="project in projects" :key="project?._id">
        <h2>{{ project?.name }}</h2>
        <h3>Project ID: {{ project?._id }}</h3>
        <p>{{ project?.description }}</p>
      </li>
    </ul>
  </div>
    
    <div class="form-container">
<!--form to create projects-->
      <div id="formcreate">
        <h2>Create a New Project</h2>
        <form @submit.prevent="createProject">
          <label for="Name">Project Name</label>
          <input type="text" v-model="newProject.name" id="Name" placeholder="Name" required />
          <br>
          <br>
          <label for="description">Project Description</label>
          <textarea v-model="newProject.description" id="description" placeholder="Description..." required></textarea>
          <br>
          <br>
          <button type="submit" id="createbtn">Create Project</button>
        </form>
      </div>


<!--form to delete projects using the id-->
      <div id="formdelete">
        <h2>Delete a Project</h2>
        <form @submit.prevent="deleteProject(projectId)">
          <label for="ID">Project ID</label>
          <input type="text" v-model="projectId" id="ID" placeholder="Project Id" required />
          <br>
          <br>
          <button type="submit" id="deletebtn">Delete Project</button>
        </form>
      </div>


<!--form to update projects-->
      <div id="formupdate">
        <h2>Update a Project</h2>
        <form @submit.prevent="updateProject(pId)">
          <label for="Id">Project ID</label>
          <input type="text" v-model="pId" id="Id" placeholder="Project Id" required />
          <br>
          <br>
          <label for="Name">Project Name</label>
          <input type="text" v-model="update.name" id="Name" placeholder="Name" required />
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

  </div>
<div class="skill-container">
    <h1>Skill</h1>
    <div class="skill-list">
      <div v-if="pending2">Loading...</div>
    <ul>
      <li v-for="skill in skill" :key="skill?._id">
        <h2>{{ skill?.name }}</h2>
        <h3>Skill ID: {{ skill?._id }}</h3>
        <p>{{ skill?.description }}</p>
      </li>
    </ul>
    </div>
  
<!--form to create skills-->
  <div class="form-container">
    <div id="formcreate">
      <h2>Create a New Skill</h2>
      <form @submit.prevent="createSkill">
        <label for="Name">Skill Name</label>
        <input type="text" v-model="newSkill.name" id="Name" placeholder="Name" required />
        <br>
        <br>
        <label for="description">Skill Description</label>
        <textarea v-model="newSkill.description" id="description" placeholder="Description..." required></textarea>
        <br>
        <br>
        <button type="submit" id="createbtn">Create Skill</button>
      </form>
    </div>


<!--form to delete skills using the id-->
    <div id="formdelete">
      <h2>Delete a Skill</h2>
      <form @submit.prevent="deleteSkill(skillId)">
        <label for="ID">Skill ID</label>
        <select v-model="skillId" id="ID" required class="dropdown">
          <option disabled value="">Select Skill ID</option>
          <option v-for="skill in skill" :key="skill._id" :value="skill._id">{{ skill._id }}</option>
        </select>
        <br>
        <br>
        <button type="submit" id="deletebtn">Delete Skill</button>
      </form>
    </div>


<!--form to update skills-->
    <div id="formupdate">
      <h2>Update a Skill</h2>
      <form @submit.prevent="updateSkill(sId)">
        <label for="Id">Skill ID</label>
        <select v-model="sId" id="Id" required class="dropdown">
          <option disabled value="">Select Skill ID</option>
          <option v-for="skill in skill" :key="skill._id" :value="skill._id">{{ skill._id }}</option>
        </select>
        <br>
        <br>
        <label for="Name">Skill Name</label>
        <input type="text" v-model="updates.name" id="Name" placeholder="Name" required />
        <br>
        <br>
        <label for="description">Skill Description</label>
        <textarea v-model="updates.description" id="description" placeholder="Description..." required></textarea>
        <br>
        <br>
        <button type="submit" id="updatebtn">Update Skill</button>
      </form>
    </div>
  </div>
</div>
</template>

<script setup>
const name = "Navoda Rajapakshe";

const { data: projects, pending1, error1 } = useFetch('http://localhost:5000/projects'); //usefetch calls for data from projects
const { data: skill, pending2, error2 } = useFetch('http://localhost:5000/skill');

const newProject = ref({
  name: '',
  description: ''
});
//create new project
const createProject = async () => {
  try {
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
  } catch (error) {
    console.error('Error creating project:', error.message);
    return false;
  }
}

//delete project
const projectId = ref('');
const deleteProject = async (_id) => {
  try {
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
  try {
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
//create new skill
const newSkill = ref({
  name: '',
  description: ''
});
const createSkill = async () => {
  try {
    const response = await fetch('http://localhost:5000/skill', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json'
      },
      body: JSON.stringify(newSkill.value)
    });
    if (!response.ok) {
      throw new Error('Failed to create skill');
    }
    //add the new skill to the list
    const skills = await response.json();
    skill.value.push(skills);

    //clear the input field
    newSkill.value.name = '';
    newSkill.value.description = '';
    return true;
  } catch (error) {
    console.error('Error creating skill:', error.message);
    return false;
  }
}
//delete skill
const skillId = ref('');
const deleteSkill = async (_id) => {
  try {
    const response = await fetch(`http://localhost:5000/skill/${_id}`, {
      method: 'DELETE'
    });
    if (!response.ok) {
      throw new Error('Failed to delete skill');
    }
    //remove the skill from the list
    const index = skill.value.findIndex(skill => skill._id === _id);
    if (index !== -1) {
      skill.value.splice(index, 1);
    }
    //clear the input field
    skillId.value = '';
  } catch (error) {
    console.error('Error deleting skill:', error.message);
  }
}
//update skill by id
const sId = ref('');
const updates = ref({
  name: '',
  description: ''
});
const updateSkill = async (_id) => {
  try {
    const response = await fetch(`http://localhost:5000/skill/${_id}`, {
      method: 'PATCH',
      headers: {
        'Content-Type': 'application/json'
      },
      body: JSON.stringify(updates.value)
    });
    if (!response.ok) {
      throw new Error('Failed to update skill');
    }
    //update the skill in the list
    const skills = skill.value.find(skill => skill._id === _id);
    if (skills) {
      skills.name = updates.value.name;
      skills.description = updates.value.description;
    }
    //clear the input field
    sId.value = '';
    updates.value.name = '';
    updates.value.description = '';
    return true;
  } catch (error) {
    console.error('Error updating skill:', error.message);
    return false;
  }
}
</script>


<style>
.navbar {
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

.navbar li a .active {
  color: #007EA7;
}
.project-container {
  display: flex;
  flex-direction: column;
  margin-bottom: 40px;
}

.skill-container {
  display: flex;
  flex-direction: column;
}
.project-list,
.skill-list {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  grid-gap: 20px;
}

.project-list li,
.skill-list li {
  list-style: none;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 5px;
  background-color: #f9f9f9;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  width: calc(50% - 20px);
}
.form-container {
  display: flex;
  flex-wrap: wrap;
  gap: 20px;
  padding: 20px;
  justify-content: space-between;
  background-color: #F6F4EB;
}

.form-container>div {
  flex: 1;
  min-width: 300px;
  padding: 20px;
  border: 1px solid #ccc;
  border-radius: 5px;
  background-color: #f9f9f9;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  transition: transform 0.2s;
}

.form-container>div:hover {
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

.dropdown {
  width: 200px;
  padding: 8px;
  font-size: 16px;
  border: 1px solid #ccc;
  border-radius: 4px;
  background-color: #fff;
  color: #333;
}

.dropdown option {
  padding: 8px;
  background-color: #fff;
  color: #333;
  font-family: 'Times New Roman', Times, serif
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
