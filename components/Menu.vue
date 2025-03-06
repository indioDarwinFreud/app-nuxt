<template>
    <nav class="navbar navbar-expand-lg bg-body-tertiary">
        <div class="container-fluid">
            <a class="navbar-brand" href="#">Navbar</a>
            <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNav"
                aria-controls="navbarNav" aria-expanded="false" aria-label="Toggle navigation">
                <span class="navbar-toggler-icon"></span>
            </button>
            <div class="collapse navbar-collapse" id="navbarNav">
                <ul class="navbar-nav">
                    <li class="nav-item">
                        <NuxtLink :to="{ name: 'index' }" class="nav-link" active-class="active">Home</NuxtLink>
                    </li>
                    <li class="nav-item">
                        <NuxtLink :to="{ name: 'product' }" class="nav-link" active-class="active">Product</NuxtLink>
                    </li>
                    <li class="nav-item">
                        <NuxtLink :to="{ name: 'contacto' }" class="nav-link" active-class="active">Contacto</NuxtLink>
                    </li>
                    <li class="nav-item">
                        <NuxtLink :to="{ name: 'login' }" class="nav-link" active-class="active">Login</NuxtLink>
                    </li>
                    <li class="nav-item">
                        <NuxtLink :to="{ name: 'challenge' }" class="nav-link" active-class="active">Challenge</NuxtLink>
                    </li>      
                </ul>
            </div>
        </div>
    </nav>
</template>
<style scoped>
.active {
    border-bottom: 2px solid blue;
}
</style>
