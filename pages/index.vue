<template>
    <div>
        <h1>Page Challenge</h1>
        <Challenge :slides="slides" :autoplay="true" :autoplayDelay="5000"/>
        
    </div>
</template>

<script>
export default {
    data() {
        return {
            slides: [] // Define slides here
        };
    }
};
</script>