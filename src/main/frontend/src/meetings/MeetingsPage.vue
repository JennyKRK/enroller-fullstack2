<template>
    <div>

<!--        <el-table :data="userInfo"> -->

<!--            &lt;!&ndash; this is an index column, ie row number starting from "1" &ndash;&gt;-->
<!--            <el-table-column type="index" min-width="50"></el-table-column>-->

<!--            &lt;!&ndash; this column shows the "id" property of your objects &ndash;&gt;-->
<!--            <el-table-column prop="id" label="ID"></el-table-column>-->

<!--            &lt;!&ndash; this column shows the "username" property of your objects &ndash;&gt;-->
<!--            <el-table-column prop="username" label="Username"></el-table-column>-->
<!--        </el-table>-->





        <NewMeetingForm @added="addNewMeeting($event)"></NewMeetingForm>
        <span v-if="meetings.length == 0">Brak zaplanowanych spotkań.</span>
        <h3 v-else>Zaplanowane zajęcia ({{ meetings.length }})</h3>
        <br>

        <MeetingsList :meetings="meetings"
                      :username="username"
                      @attend="addMeetingParticipant($event)"
                      @unattend="removeMeetingParticipant($event)"
                      @delete="deleteMeeting($event)"
                      @info="getMoreInfo($event)"
                  ></MeetingsList>
    </div>
</template>

<script>
let id =1;
import NewMeetingForm from "./NewMeetingForm";
import MeetingsList from "./MeetingsList";
import axios, {Axios} from "axios";
import {store} from "@/meetings/store";
export default {
    computed: {
        store() {
            return store
        }
    },
    components: {NewMeetingForm, MeetingsList},
    props: {username: String,
 },
    data() {
        return {
            meetings: [],
            participantsList: [],

        };
    },
     created(){
        this.getInfo();
    },

    methods: {

        getInfo(){

            let a = 0;

            while (store.count > a) {
                a++;

                let listParticipants = [];
                axios.get('api/meetings/' + a + '/participants').then(response => {
                    for (let i = 0 ; i < response.data.length; i++){
                        let newP = response.data[i].login;
                        listParticipants.push(newP);
                    }                })

                axios.get('api/meetings/' + a).then(response => this.meetings.push({id: response.data.id,title:response.data.title, description: response.data.description,
                participants: listParticipants}))

                    .catch(err => {
                    console.log("Err");
                });

            }
        }
        ,

        log(item) {
            console.log(item)
        },


        addNewMeeting(meeting) {

            this.meetings.push({id: id++, title: meeting.title, description: meeting.description,participants: []})

            axios.post('/api/meetings', meeting)
                .then(response => {
                            console.log(response.data.id);
                            console.log(response.data);

                        })
                            .catch(function (error) {
                                console.log(error);
                            });

        },

        addMeetingParticipant(meeting) {

            meeting.participants.push(this.username);
            let b = meeting.id;
            let a = this.username;
            let theurl = 'api/meetings/' + b + '/participants/';
            axios.get('api/participants/' + a).then(response =>
                axios.post(theurl,response.data)
            );

        },

        removeMeetingParticipant(meeting) {
            meeting.participants.splice(meeting.participants.indexOf(this.username), 1);
            let a = this.username;
            let b = meeting.id;
            let theurl = 'api/meetings/' + b + '/participants/' + a;
            axios.get('api/participants/' + a).then(response =>
                axios.delete(theurl)
            );
        },
        deleteMeeting(meeting) {
            this.meetings.splice(this.meetings.indexOf(meeting), 1);
            let b = meeting.id;
            axios.delete('api/meetings/' + b).then(response => console.log(response.data)).catch(err => {
                console.log("Error");})
        },















        getInfo7() {
            let a = 1;
            let list1 = [];
            axios.get('api/meetings/' + a + '/participants').then(response => {
                console.log(response.data[0].login);
            });
        }
        ,
    },

}
</script>
