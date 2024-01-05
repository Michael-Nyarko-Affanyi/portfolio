<template>
    <div class="text-white">
        <nav class="w-full text-green-600 font-medium py-5">
            <ul class="flex justify-center">
                <li>
                    <router-link to="/">Home</router-link>
                </li>
                <li>
                    <router-link to="/about">Skills</router-link>
                </li>
                <li>
                    <router-link to="/projects">Projects</router-link>
                </li>
                <li>
                    <router-link to="/contact">Contact</router-link>
                </li>
            </ul>
        </nav>
        <div>
            <img src="" alt="">
        </div>
    </div>
</template>

<script>

export default {

}
</script>

<style>

li {
    transition: all 0.2s ease-in-out;
    padding: 8px 16px;
    border-radius: 8px;
}

li:hover {
    background-color: rgba(0, 255, 0, 0.1);
    cursor: pointer;
}
</style>