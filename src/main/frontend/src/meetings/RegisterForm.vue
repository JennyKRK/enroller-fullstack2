<template>


    <table>
        <thead>
        <tr>
            <th></th>
            <th>ID</th>
            <th>w bazie {{ meetings2.length }}</th>
        </tr>
        </thead>
        <tbody>
        <tr v-for="meeting in meetings2">
            <td>{{ meeting.title}}</td>
        </tr>
        </tbody>
    </table>

</template>

<script>

import axios from "axios";

export default {
    data() {
        return {
            meetings2: [],
        };
    },

    created(){
        //axios.get('/api/meetings').then(response => console.log(response.data));

    }}




</script>

<style scoped>

</style>


