<template>
    <div class="loading">
        <svg version="1.1" id="L2" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" x="0px" y="0px"
  viewBox="0 0 100 100" enable-background="new 0 0 100 100" xml:space="preserve">
<circle fill="none" stroke="#B2CA59" stroke-width="4" stroke-miterlimit="10" cx="50" cy="50" r="48"/>
<line fill="none" stroke-linecap="round" stroke="#B2CA59" stroke-width="4" stroke-miterlimit="10" x1="50" y1="50" x2="85" y2="50.5">
  <animateTransform 
       attributeName="transform" 
       dur="2s"
       type="rotate"
       from="0 50 50"
       to="360 50 50"
       repeatCount="indefinite" />
</line>
<line fill="none" stroke-linecap="round" stroke="#B2CA59" stroke-width="4" stroke-miterlimit="10" x1="50" y1="50" x2="49.5" y2="74">
  <animateTransform 
       attributeName="transform" 
       dur="15s"
       type="rotate"
       from="0 50 50"
       to="360 50 50"
       repeatCount="indefinite" />
</line>
</svg>
    </div>
</template>
<script>
    export default{
        name:"Loading"
    }
</script>
<style scoped>
    .loading{
        display:flex;
        justify-content: center;
        align-items: center;
        width:100vw;
        height:100vh;
        margin:-.5em;
        z-index:3;
        position:absolute;
        background-color: rgb(49,46,46);
    }
    .loading div{
        display:block;
        margin:auto;
    }
    svg{
        height:50vh;
    }
</style>