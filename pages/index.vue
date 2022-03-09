<template>
    <div id="app" :class="{'theme-dark': nightMode}">
        <div class="users__wrap" :class="{'position-table': changePos}">
            <i class="position fa-solid fa-align-justify" v-if="changePos" @click="changePos = !changePos"></i>
            <i class="position fa-solid fa-table-cells" v-else @click="changePos = !changePos"></i>
            <i class="theme fas fa-sun" v-if="nightMode" @click="nightMode = !nightMode"></i>
            <i class="theme fas fa-moon" v-else @click="nightMode = !nightMode"></i>
            <div class="users" v-for="user in users" :key="user.id.value">
                <h4>{{user.name.first}} {{user.name.last}}</h4>
                <p>City:{{user.location.city}}</p>
                <p>Nationality: {{user.nat}}</p>
            </div>
        </div>
    </div>
</template>
<script>
import axios from "axios";
import { stringify } from 'querystring';
export default {
    name: "IndexPage",
    
    data() {
        return {
            users: [],
            nightMode: false,
            changePos: false
        };
    },
    watch:{
        nightMode: function(){
            localStorage.setItem("nightMode", JSON.stringify(this.nightMode))
            
        },
        changePos: function(){
            localStorage.setItem("changePos", JSON.stringify(this.changePos))
        },
        
    },
    mounted() {
        axios
        .get("https://randomuser.me/api/?results=20")
        .then((response) => (this.users = response.data.results))
        .catch((error) => console.log(error));
        this.nightMode = JSON.parse(localStorage.getItem("nightMode"))
        this.changePos = JSON.parse(localStorage.getItem("changePos"))
    }
}
</script>
<style>
    @import url("https://use.fontawesome.com/releases/v6.0.0/css/all.css");
    #app{
        transition: all .5s ease-in-out;
        padding-top: 40px;
    }
    .users__wrap{
        max-width: 850px;
        width: 100%;
        margin: 0 auto;
    }
    .users{
        width: 200px;
        margin: 10px auto;
        text-align: center;
        border: 2px solid black;
        border-radius: 10px;

    }
    .theme {
        position: fixed;
        top: 10px;
        right: 40px;
        font-size: 25px;
        cursor: pointer;
    }
    .position{
        font-size: 25px;
        cursor: pointer;
        position: fixed;
        top: 10px;
        left: 40px;
    }
    .fa-sun {
        color: yellow;
    }
    .theme-dark {
        color: white;
        background-color: rgb(81, 88, 88);
    }
    .position-table{
        display: flex;
        flex-wrap: wrap;
        justify-content: space-between;
    }
</style>
