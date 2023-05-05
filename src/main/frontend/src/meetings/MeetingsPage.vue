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
        <span v-if="meetingsToBeDisplayed.length == 0">Brak zaplanowanych spotkań.</span>
        <h3 v-else>Zaplanowane zajęcia ({{ meetingsToBeDisplayed.length }})</h3>
        <br>
        <button @click="reloadTheList">Odswiez strone</button>
<!--        <h3> test {{ store.count }}</h3>-->
        <MeetingsList :meetings="meetingsToBeDisplayed"
                      :username="username"
                      @attend="addMeetingParticipant($event)"
                      @unattend="removeMeetingParticipant($event)"
                      @delete="deleteMeeting($event)"
                      @info="getMoreInfo($event)"
                  ></MeetingsList>



    </div>
</template>

<script>
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
            meetings2: [],
            meetingsToBeDisplayed: [],
        //     this part needs to be reworked, now the meeting list gets cleared
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
                axios.get('api/meetings/' + a).then(response => this.meetingsToBeDisplayed.push(response.data)).catch(err => {



                        console.log("Err");


                });

            }


        }
        ,

        log(item) {
            console.log(item)
        },




        pushitem(a) {
            axios.get('api/meetings/' + a).then(response => console.log(response.data)).catch(err => {
                console.log("Error");})
        },

        addNewMeeting(meeting) {

            meeting.participants = Array();


            this.meetings.push(meeting)
            this.meetings2.push(meeting)
            this.meetingsToBeDisplayed.push(meeting)



            axios.post('/api/meetings', meeting)
                .then(response => {
                            //this part is important, it gives you id
                            console.log(response.data.id);
                            console.log(response.data);

                        })
                            .catch(function (error) {
                                console.log(error);
                            });







        },

        addMeetingParticipant(meeting) {
            // meeting.participants.push(this.username);
            //

            let b = meeting.id;
            //alert(meeting.id);
            let a = this.username;
            let theurl = 'api/meetings/' + b + '/participants/';
            axios.get('api/participants/' + a).then(response =>
                axios.post(theurl,response.data)
            );

        },




        removeMeetingParticipant(meeting) {
            meeting.participants.splice(meeting.participants.indexOf(this.username), 1);
        },
        deleteMeeting(meeting) {
            this.meetings.splice(this.meetings.indexOf(meeting), 1);
            let b = meeting.id;
            alert("Odswiez strone")
            axios.delete('api/meetings/' + b).then(response => console.log(response.data)).catch(err => {
                console.log("Error");})
            // axios.delete('/api/meetings/2')
        },

        reloadTheList() {
            this.meetingsToBeDisplayed = [];
            this.getInfo();
        },




}}
</script>
