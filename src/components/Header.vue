<template>
    <nav class="navbar navbar-light">
        <div class="container">
            <a class="navbar-brand" href="#">Navbar</a>
            <ul class="nav nav-pills">
                <li class="nav-item">
                    <router-link class="nav-link" to="/">Home</router-link>
                </li>
                <li class="nav-item">
                    <router-link class="nav-link" to="/cart">Cart</router-link>
                </li>
            </ul>
        </div>
    </nav>
</template>

<script>
export default {
  name: "Header",
};
</script>

<style scoped>
nav {
    background: black;
}

a {
    color: white !important;
}

.active {
    background: white !important;
    color: black !important;
}
</style>

