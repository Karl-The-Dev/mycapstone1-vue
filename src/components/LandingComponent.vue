<script setup>

    import landing from "../data/landing.json";

    const test = landing[0];

    console.log(test)

</script>

<template>
    <div class="container-fluid py-md-5 mt-5" id="landing">
        <div class="row justify-content-center align-items-center py-md-5 my-md-5">
            <!-- Image Column -->
            <div class="col-md-4 col-lg-3 text-center text-md-start mb-4 mb-md-0">
                <img src="/images/dev2.jpg" class="img-fluid w-100 rounded-circle profile-img" alt="Developer Profile Picture">
            </div>
            
            <!-- Text Content Column -->
            <div class="col-md-7 col-lg-6">
                <h1 id="text-title">{{test.name}}</h1>
                <h3>{{test.title}}</h3>
                
                <!-- Description with collapsible functionality -->
                <div class="description-container">
                    <input type="checkbox" id="toggle-description" class="toggle-checkbox">
                    <div class="description-content">
                        <div v-for="des in test.description" :key="des">
                            <p>{{des}}</p>
                        </div>
                    </div>
                    <label for="toggle-description" class="toggle-label text-warning">View Description</label>
                </div>
            </div>
        </div>
    </div>
</template>

<style scoped>
#landing {
    background-color: var(--bs-dark);
    color: white
}

.profile-img {
    max-width: 280px;
    border: 4px solid var(--bs-primary);
}

h3 {
    color: var(--bs-gray-400);
}

p {
    color: var(--bs-gray-300);
    line-height: 1.7;
}

/* Collapsible functionality - Mobile Only */
@media (max-width: 768px) {
    .toggle-checkbox {
        display: none;
    }
    
    .description-content {
        max-height: 80px;
        overflow: hidden;
        transition: max-height 0.3s ease-out;
        position: relative;
    }
    
    .description-content::after {
        content: '';
        position: absolute;
        bottom: 0;
        left: 0;
        width: 100%;
        height: 40px;
        background: linear-gradient(transparent, var(--bs-dark));
        pointer-events: none;
    }
    
    .toggle-checkbox:checked ~ .description-content {
        max-height: 500px;
    }
    
    .toggle-checkbox:checked ~ .description-content::after {
        display: none;
    }
    
    .toggle-label {
        display: inline-block;
        color: var(--bs-primary);
        font-weight: 600;
        cursor: pointer;
        margin-top: 10px;
        padding: 5px 0;
    }
    
}

/* Hide toggle on desktop */
@media (min-width: 769px) {
    .toggle-checkbox,
    .toggle-label {
        display: none;
    }
    
    .description-content {
        max-height: none;
        overflow: visible;
    }
    
    .description-content::after {
        display: none;
    }
}
</style>