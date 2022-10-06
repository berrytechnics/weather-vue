<template>
    <div class="forecast">
        <div class="header">
            <h1>Forecast</h1>
            <div class="container">
                <div class="box" v-for="forecast in weather">
                    <h5>{{new Date(forecast.dt*1000).toLocaleDateString()}}</h5>
                    <div class="img">
                        <img :src="iconURL+forecast.weather[0].icon+'@2x.png'" /><br />
                        <table>
                            <tbody>
                                <tr>
                                    <td>Low-High</td>
                                    <td class="text-right">{{forecast.temp.min}}&deg;-{{forecast.temp.max}}&deg;</td>
                                </tr>
                                <tr>
                                    <td>Humidity</td>
                                    <td class="text-right">{{forecast.humidity}}%</td>
                                </tr>
                                <tr>
                                    <td>Pressure</td>
                                    <td class="text-right">{{forecast.pressure}}"/Hg</td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>
<script>
    export default {
        name:"Forecast",
        components:[
        ],
        props:{
            weather:Object,
            location:Object
        },
        data(){
            return{
                iconURL: "http://openweathermap.org/img/wn/"
            }
        }
    }
</script>
<style scoped>
    .container{
        display: flex;
        flex-wrap:wrap;
        gap:1rem;
        justify-content: center;
    }
    .box{
        border:1px black solid;
        border-radius:1rem;
        padding:.5rem;
        box-shadow: .5rem .5rem .8rem rgba(0, 0, 1, 0.319);
    }
    h1,h5{
        text-align:center
    }
    img{
        display:block;
        margin:auto;
    }
</style>