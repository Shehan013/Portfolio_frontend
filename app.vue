<template>
  <div>
    <h1>Shehan Lokuwella</h1>
    <p>IT Undergraduate of University of Moratuwa</p>
  </div>
  <div>
    <h2>{{ name }}</h2>
    <ul>
      <li v-for="project in projects" :key="project.name"> <!--key="project._id recommend when implemeting update project by id-->
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
  <div>
    <form @submit.prevent="SubmitupdateProject">
      <h2>Update Project</h2>
      <div>
        <label for="projectId">Project ID:</label>
        <input type="text" id="projectId" v-model="updateProject.id" required>
      </div><br>
      <div>
        <label for="projectName">Project Name:</label>
        <input type="text" id="projectName" v-model="updateProject.name" required>
      </div><br>
      <div>
        <label for="projectDescription">Project Description:</label>
        <textarea id="projectDescription" v-model="updateProject.description" required></textarea>
      </div><br>
      <button type="submit">Update Project</button>
    </form>
  </div>
  <br><br>
  <!--<div>
    <form @submit.prevent="deleteProject">
      <div>
        <label for="projectId">Project ID:</label>
        <input type="text" id="projectId" v-model="deleteProjectId" required>
      </div><br>
      <button type="submit">Delete Project</button>
    </form>
  </div>-->
  <div>
    <form @submit.prevent="deleteProject">
      <div>
        <label for="projectId">Select Project Name:</label>
        <select id="projectId" v-model="deleteProjectId" required>
          <option v-for="project in projects" :key="project._id" :value="project._id">
            {{ project.name }}
          </option>
        </select>
      </div><br>
      <button type="submit">Delete Project</button>
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

  // Update project by id using patch method
  const updateProject = ref({
    id: '',
    name: '',
    description: ''
  });
  const SubmitupdateProject = async () => {
    try {
      console.log('Updating project with ID:', updateProject.value.id) //Debug
      const response = await fetch(`http://localhost:5000/projects/${updateProject.value.id}`, {
        method: 'PATCH',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify({
          name: updateProject.value.name,
          description: updateProject.value.description
      }),
      });
      if (!response.ok) {
        throw new Error('Failed to update project');
      }
      // Fetch the updated project
      const result = await response.json();
      console.log('Updated project:', result); //Debug

      // Find the index of the updated project in the projects array
      const index = projects.value.findIndex(project => project._id === result._id);
      if (index !== -1) {
        // Update the project in the projects array
        projects.value[index] = { ...result };
      }
      else {
        console.warn('Project not found in the projects array.'); //Debug
      }
      // Clear the form inputs
      updateProject.value.id = '';
      updateProject.value.name = '';
      updateProject.value.description = '';
    } catch (error) {
      console.error(error);
    }
  };

  //Delete a project by id
  const deleteProjectId = ref('');

  const deleteProject = async () => {
    try {
      console.log('Deleting project with ID:', deleteProjectId.value); // Debug
      const response = await fetch(`http://localhost:5000/projects/${deleteProjectId.value}`, {
        method: 'DELETE',
      });
      if (!response.ok) {
        throw new Error('Failed to delete project');
      }
      // Remove the deleted project from the projects array
      projects.value = projects.value.filter(project => project._id !== deleteProjectId.value);
      // Clear the form input
      deleteProjectId.value = '';
    } catch (error) {
      console.error(error);
    }
  };
   
</script>

<style></style>