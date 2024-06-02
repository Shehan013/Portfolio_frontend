<template>
  <div>
    <h1>Shehan Lokuwella</h1>
    <p>IT Undergraduate of University of Moratuwa</p>
  </div>
  <div>
    <h2>{{ name }}</h2>
    <ul>
      <li v-for="project in projects" :key="project.name">
        <h3>{{ project.name }}</h3>
        <p>{{ project.description }}</p>
      </li>
    </ul>
      
      <h2>Blogs</h2>
      <!--code for blogs-->
    <ul>
      <li v-for="blog in blogs" :key="blog.title">
        <h3>{{ blog.title }}</h3>
        <p>{{ blog.content }}</p>
      </li>
    </ul>
  </div>
  <div>
    <form @submit.prevent="createProject">
      <h2>Create New Project</h2>
      <div>
      <label for="projectName">Project Name:</label>
      <input type="text" id="projectName" v-model="newProject.name" required>
      </div><br>
      <div>
      <label for="projectDescription">Project Description:</label>
      <textarea id="projectDescription" v-model="newProject.description" required></textarea>
    </div><br>
      <button type="submit">Create Project</button>
    </form>
  </div>
</template>

// add script and style tags

<script setup>

const name = 'Projetcs';
/*const projects = [
   {
    name : 'Project 1',
    description : 'This is project 1'
   },

   {
    name : 'Project 2',
    description : 'This is project 2'
   },
  ];*/
  const {data: projects, pending, error} = useFetch('http://localhost:5000/projects');
  
  //code for blogs
  const {data: blogs, pending2, error2} = useFetch('http://localhost:5000/blogs');

  const newProject = ref({
    name: '',
    description: ''
  });

  const createProject = async () =>{
    console.log( newProject.value);
    try {
      const response = await fetch('http://localhost:5000/projects', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify(newProject.value),
      });

      if (!response.ok) {
        throw new Error('Failed to create project');
      }

      // Fetch the updated list of projects
      const result = await response.json();
      projects.value.push(result);

      // Clear the form inputs
      newProject.value.name = '';
      newProject.value.description = '';

    } catch (error) {
      console.error(error);
    }
  }
</script>

<style></style>