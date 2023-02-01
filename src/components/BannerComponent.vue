<template>
    <div id="main-banner">
        <h1>Make Your Burger</h1>
    </div>
</template>

<script>
export default {
   name: "BannerComponent"
}
</script>

<style scoped>
    
    #main-banner {
        background-image: url('/public/img/burger.jpg');
        background-position: 0 -50px;
        background-size: cover;
        height: 500px;
        display: flex;
        align-items: center;
        justify-content: flex-start;
    }

    #main-banner h1 {
        color: #FCBA03; /** #FCBA03 **/ /** #DA291C **/
        text-align: center;
        font-size: 50px;
        margin-left: 20px;
        padding: 2px 20px;
        background-color: #222;
        transition: .5s;
        text-decoration: overline;
    }

    #main-banner h1:hover {
        color: #DA291C;
        text-decoration: overline;
    }

</style>