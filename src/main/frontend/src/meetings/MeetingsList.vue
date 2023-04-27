<template>
    <table v-if="meetings.length > 0">
        <thead>
        <tr>
            <th>Nazwa spotkania</th>
            <th>Opis</th>
            <th>ID</th>
            <th>Zapisani</th>
            <th>Usun</th>
            <th>Akcje</th>

<!--            <th>ID</th>-->
        </tr>
        </thead>
        <tbody>
        <tr v-for="meeting in meetings" :key="meeting.title">
            <td>{{ meeting.title }}</td>
            <td>{{ meeting.description }}</td>
            <td>{{meeting.id}}</td>
            <td>
                <ul v-if="meeting.participants">
                    <li v-for="participant in meeting.participants" :key="participant">
                        {{ participant }}
                    </li>
                </ul>
            </td>
            <td>
                <button class="button" @click="$emit('delete', meeting)">
                    Usuń spotkanie</button>
            </td>
            <td style="text-align: right; min-width: 400px" v-if="meeting.participants">
                <button v-if="meeting.participants.indexOf(username) < 0"
                        class="button-outline"
                        @click="$emit('attend', meeting)">
                    Zapisz się
                </button>
                <button v-else class="button-outline" @click="$emit('unattend', meeting)">Wypisz się</button>
<!--                <button v-if="meeting.participants.length === 0" class="button" @click="$emit('delete', meeting)">-->
<!--                    Usuń puste spotkanie-->
<!--                </button>-->


                    <!--                <button>{{meeting.name}}</button>-->

            </td>
<!--            <td>{{meeting.id}}</td>-->
        </tr>
        </tbody>
    </table>
</template>

<script>
export default {
    props: {
        meetings: Array,
        meetingsOld:Array,
        username: String,
    },
}
</script>