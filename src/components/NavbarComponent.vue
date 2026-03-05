<script setup>

</script>

<template>
    <nav class="navbar px-md-5 fixed-top bg-warning mb-5" id="nav">
        <div class="container-fluid px-md-5 d-flex justify-content-lg-center">
            <!-- Hamburger Button -->
            <button class="hamburger-btn" @click="toggleMenu" type="button">
                <span class="hamburger-icon"></span>
                <span class="hamburger-icon"></span>
                <span class="hamburger-icon"></span>
            </button>	    
            
            <!-- Navigation Menu -->
            <div class="nav-menu " :class="{ 'show': isMenuOpen }">
                <ul class="nav-links">
                    <li class="nav-item">
                        <a class="nav-link active" aria-current="page" href="#landing">Home</a>
                    </li>
                    <li class="nav-item">
                        <a class="nav-link" href="#projects">My Projects</a>
                    </li>                    
                    <li class="nav-item">
                        <a class="nav-link" href="#tools">Tools</a>
                    </li>
                    <li class="nav-item">
                        <a class="nav-link" href="#contact">Contact</a>
                    </li>
                </ul>
            </div>
        </div>
    </nav>
</template>

<script setup>
import { ref } from 'vue';

const isMenuOpen = ref(false);

const toggleMenu = () => {
    isMenuOpen.value = !isMenuOpen.value;
};
</script>

<style scoped>
.navbar {
    background-color: #ffc107 !important; /* bg-warning */
    padding: 1rem 0;
    z-index: 1000;
    box-shadow: 0 2px 10px rgba(0,0,0,0.1);
}

.container-fluid {
    display: flex;
    justify-content: space-between;
    align-items: center;
    position: relative;
}

/* Hamburger Button */
.hamburger-btn {
    display: none;
    background: none;
    border: none;
    cursor: pointer;
    padding: 10px;
    z-index: 1001;
}

.hamburger-icon {
    display: block;
    width: 25px;
    height: 3px;
    background-color: #333;
    margin: 5px 0;
    transition: all 0.3s ease;
}

/* Navigation Menu */
.nav-menu {
    display: flex;
    align-items: center;
}

.nav-links {
    display: flex;
    list-style: none;
    margin: 0;
    padding: 0;
    gap: 2rem;
}

.nav-item {
    margin: 0;
}

.nav-link {
    color: #333;
    text-decoration: none;
    font-weight: 500;
    padding: 0.5rem 1rem;
    transition: color 0.3s ease;
}

.nav-link:hover,
.nav-link.active {
    color: #000;
    background-color: rgba(0,0,0,0.05);
    border-radius: 4px;
}

/* Media Query for Mobile Devices */
@media (max-width: 768px) {
    .hamburger-btn {
        display: block;
    }

    .nav-menu {
        position: fixed;
        top: 70px;
        left: 0;
        width: 100%;
        background-color: #ffc107;
        padding: 0;
        max-height: 0;
        overflow: hidden;
        transition: max-height 0.3s ease-out;
        box-shadow: 0 4px 6px rgba(0,0,0,0.1);
    }

    .nav-menu.show {
        max-height: 300px;
        padding: 1rem 0;
    }

    .nav-links {
        flex-direction: column;
        align-items: center;
        gap: 0.5rem;
    }

    .nav-item {
        width: 100%;
        text-align: center;
    }

    .nav-link {
        display: block;
        padding: 1rem;
        width: 100%;
    }

    .nav-link:hover {
        background-color: rgba(0,0,0,0.1);
    }

    /* Hamburger animation when menu is open */
    .hamburger-btn[aria-expanded="true"] .hamburger-icon:nth-child(1) {
        transform: rotate(45deg) translate(5px, 5px);
    }

    .hamburger-btn[aria-expanded="true"] .hamburger-icon:nth-child(2) {
        opacity: 0;
    }

    .hamburger-btn[aria-expanded="true"] .hamburger-icon:nth-child(3) {
        transform: rotate(-45deg) translate(7px, -7px);
    }
}

/* Larger screens */
@media (min-width: 769px) {
    .nav-menu {
        display: flex !important;
        max-height: none !important;
        position: static;
        width: auto;
        box-shadow: none;
    }
}

/* Ensure proper spacing on mobile */
@media (max-width: 576px) {
    .navbar {
        padding: 0.5rem 0;
    }
    
    .hamburger-btn {
        padding: 5px;
    }
}
</style>
