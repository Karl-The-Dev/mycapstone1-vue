<script setup>

import {computed} from "vue";
import ProjectCard from "./ProjectCard.vue";
import projects from "../data/projects.json";

const chunkSize = 3;

const chunkedProjects = computed(() => {

    const chunks = [];
    //              index 0         index 1       index 2
    // chucnks = [ [ {0},{1},{2}], [{3},{4},{5}], [{6},{7},{8}]]

    for(let i = 0; i< projects.length; i+=chunkSize){
        //                         0, 3 (meaning: index 0,1,2)
        // pushing 3 items at a time to chunks
        // i=3
        //                          3, 6 (meaning: index 3,4,5)
        // i=6
        //                          6,9 (meaning: index 6,7,8)
        chunks.push(projects.slice(i, i+chunkSize));
    }
    return chunks
    
})

</script>

<template>
    <div class="container pb-5" id="projects">
        <h1 class="text-center p-5 text-primary">My Projects</h1>
		<div class="row my-4 text-center" id="services">
            
			<div 
	    	v-for="(group, index) in chunkedProjects"
	    	:key="index"
	    	class="card-deck my-5 justify-content-center"
	    >
	    	<ProjectCard 
	    		v-for="project in group"
	    		:key="project.id"
	    		:project="project"
	    	/>
	    	
	    </div>
			

		</div>

	</div>
</template>

<style scoped>
    
</style>