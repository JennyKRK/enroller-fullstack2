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
        <MeetingsList :meetings="meetingsToBeDisplayed"
                      :username="username"
                      @attend="addMeetingParticipant($event)"
                      @unattend="removeMeetingParticipant($event)"
                      @delete="deleteMeeting($event)"></MeetingsList>

<!--        <MeetingsList2 :meetingsOld="meetingsToBeDisplayed"-->
<!--                      :username="username"-->
<!--                      @attend="addMeetingParticipant($event)"-->
<!--                      @unattend="removeMeetingParticipant($event)"-->
<!--                      @delete="deleteMeeting($event)"></MeetingsList2>-->

    </div>
</template>

<script>
import NewMeetingForm from "./NewMeetingForm";
import MeetingsList from "./MeetingsList";
import axios, {Axios} from "axios";
import MeetingList2 from "@/meetings/MeetingList2.vue";
export default {
    components: {NewMeetingForm, MeetingsList, MeetingList2},
    props: {username: String},
    data() {
        return {
            meetings: [],
            meetings2: [],
            meetingsToBeDisplayed: [],
        //     this part needs to be reworked, now the meeting list gets cleared
        };
    },
    // mounted: function() {
    //     axios.get('/api/meetings').then((res) => {
    //         localStorage.setItem("apiData", JSON.stringify(res.data));
    //     });
    //     console.log(response.data);
    // },
    created(){
        this.getInfo();
    },

    methods: {
        getInfo(){

            // for (let i = 1; i < 9; i++) {
            //     console.log(i);
            //     this.pushitem(i)
            // }
            // axios.get(`/api/meetings`)
            //     .then(response =>
            //         console.log(response.data)),
            //this part - works :)))))))))))))))))) why?
            //
            // for (var x=1; x<11; x++) {
            //     axios.get('api/meetings/' + x).then(response => console.log(response.data)).catch(err => {
            //         console.log("Error");
            //     });
            // }




            // let a = 1;

            //
            // // axios.get('api/meetings/' + a).then(response => console.log(response.data)).catch(err => {
            // //     console.log("Error");
            // // });
            //
            axios.get('api/meetings/1').then(response => this.meetingsToBeDisplayed.push(response.data))
                .catch(err => {
                    console.log("Error");
                });

            axios.get('api/meetings/2').then(response => this.meetingsToBeDisplayed.push(response.data))
                .catch(err => {
                    console.log("Error");
                });

            axios.get('api/meetings/3').then(response => this.meetingsToBeDisplayed.push(response.data))
                .catch(err => {
                    console.log("Error");
                });

            axios.get('api/meetings/4').then(response => this.meetingsToBeDisplayed.push(response.data))
                .catch(err => {
                    console.log("Error");
                });

            axios.get('api/meetings/5').then(response => this.meetingsToBeDisplayed.push(response.data))
                .catch(err => {
                    console.log("Error");
                })
            // axios.get('api/meetings/1').then(response => console.log(response.data))
            //     .catch(err => {
            //     console.log("Error");
            // });
            //
            // axios.get('api/meetings/1').then(response => console.log(response.data.id)).catch(err => {
            //     console.log("Error");
            // });
            // axios.get('api/meetings/1').then(response => console.log(response.data.title)).catch(err => {
            //     console.log("Error");
            // });
            // axios.get('api/meetings/1').then(response => console.log(response.data.description))
            // axios.get('api/meetings/2').then(response => console.log(response.data))
            // axios.get('api/meetings/3').then(response => console.log(response.data))

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
            this.meetings.push(meeting)
            this.meetings2.push(meeting)
            this.meetingsToBeDisplayed.push(meeting)

            axios.post('/api/meetings', meeting)
                .then(response => {
                            //this part is important, it gives you id
                            console.log(response.data.id);
                            console.log(response.data);
                            // alert(response.data.id);

                        })
                            .catch(function (error) {
                                console.log(error);
                            });


            //alert( meeting.title + meeting.description + meeting.id)
            // axios.post("/api/meetings", meeting)
            //     .then(response => this.meetingId = response.data.id);





            //meeting.name
            //meeting.descrition
        },
        addMeetingParticipant(meeting) {
            meeting.participants.push(this.username)
            //axios.post('api/meetings/{id}/participants',this.username)
        },
        removeMeetingParticipant(meeting) {
            meeting.participants.splice(meeting.participants.indexOf(this.username), 1);
        },
        deleteMeeting(meeting) {
            this.meetings.splice(this.meetings.indexOf(meeting), 1);
            //axios.delete('/api/meetings/2')

            //axios.delete('/api/meetings/'+this.meetings.indexOf(meeting))
        },

}}
</script>
