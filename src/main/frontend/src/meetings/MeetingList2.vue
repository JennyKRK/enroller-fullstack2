<!--<template>-->
<!--    <table v-if="meetingsOld.length> 0">-->
<!--        <thead>-->
<!--        <tr>-->
<!--            <th>Nazwa spotkania</th>-->
<!--            <th>Opis</th>-->
<!--            <th>Zapisani</th>-->
<!--            <th>Akcje</th>-->
<!--            <th>ID</th>-->
<!--        </tr>-->
<!--        </thead>-->
<!--        <tbody>-->
<!--        <tr v-for="meeting in meetingsOld" :key="meeting.title">-->
<!--            <td>{{ meeting.title }}</td>-->
<!--            <td>{{ meeting.description }}</td>-->
<!--            <td>-->
<!--                <ul v-if="meeting.participants">-->
<!--                    <li v-for="participant in meeting.participants" :key="participant">-->
<!--                        {{ participant }}-->
<!--                    </li>-->
<!--                </ul>-->
<!--            </td>-->
<!--            <td style="text-align: right; min-width: 400px" v-if="meeting.participants">-->
<!--                <button v-if="meeting.participants.indexOf(username) < 0"-->
<!--                        class="button-outline"-->
<!--                        @click="$emit('attend', meeting)">-->
<!--                    Zapisz się-->
<!--                </button>-->
<!--                <button v-else class="button-outline" @click="$emit('unattend', meeting)">Wypisz się</button>-->
<!--                <button v-if="meeting.participants.length === 0" class="button" @click="$emit('delete', meeting)">-->
<!--                    Usuń puste spotkanie-->
<!--                </button>-->
<!--                &lt;!&ndash;                <button>{{meeting.name}}</button>&ndash;&gt;-->
<!--                &lt;!&ndash;                <button>{{meeting.description}}</button>&ndash;&gt;-->
<!--            </td>-->
<!--            <td>{{meeting.id}}</td>-->
<!--        </tr>-->
<!--        </tbody>-->
<!--    </table>-->
<!--</template>-->

<!--<script>-->
<!--export default {-->
<!--    props: {-->
<!--        meetingsOld: Array,-->
<!--        username: String,-->
<!--    },-->
<!--}-->
<!--</script>-->