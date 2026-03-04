<script setup>

import { defineProps } from "vue";

	defineProps({
    project: Object
});


</script>



<template>

    <div class="card col-md-3">
        <!-- : directive to bind pohot to the src element -->
	    <img :src="project.image" class="card-img-top card-img" alt="...">
	    <div class="card-body">
	    <h5 class="card-title">{{project.title}}</h5>
	    <p class="card-text">
	       {{project.description}}
	    </p>
	    
	    </div>
	</div>


</template>