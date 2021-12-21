<template>
  <app-layout title="Chat">
    <template #header>
        <h2 class="font-semibold text-xl text-gray-800 leading-tight">
            <chat-room-selection
            v-if="currentRoom.id"
            :rooms="chatRooms"
            :currentRoom="currentRoom"
            v-on:roomChanged="setCurrenRoom($event)"
            />
        </h2>
    </template>

    <div class="py-12">
      <div class="max-w-7xl mx-auto sm:px-6 lg:px-8">
        <div class="bg-white overflow-hidden shadow-xl sm:rounded-lg">
          <message-container :messages="messages"/>
          <input-message
            :room="currentRoom"
            v-on:messageSent="getAllMessages()"
          />
          <!-- passing the current room to the input (widget (let's call it widget for now)) -->
        </div>
      </div>
    </div>
  </app-layout>
</template>

<script>
import { defineComponent } from "vue";
import AppLayout from "@/Layouts/AppLayout.vue";
import MessageContainer from "./messageContainer.vue";
import InputMessage from "./inputMessage.vue";
import ChatRoomSelection from './chatRoomSelection.vue'

export default defineComponent({
  components: {
    AppLayout,
    MessageContainer,
    InputMessage,
    ChatRoomSelection
  },
  data: function () {
    return {
      chatRooms: [],
      currentRoom: [],
      messages: [],
    };
  },
//   watch : {
//       currentRoom(val, oldVal) {
//           if(oldVal.id){
//               this.disconnect(oldVal)
//           }
//           this.connect()
//       }
//   },
  methods: {
    // connect(){
    //     if(this.currentRoom.id){
    //         let vm = this
    //         this.getAllMessages();
    //         window.Echo.private('chat.'+this.currentRoom.id)
    //         .listen('.message.new',  function(e) {
    //             vm.getAllMessages()
    //         })
    //     }
    // },
    // disconnect(room){
    //     window.Echo.leave("chat."+room.id)
    // },
    getAllRooms() {
      axios
        .get("/chat/rooms")
        .then(response => {
          this.chatRooms = response.data;
          this.setCurrenRoom(response.data[0]);
        })
        .catch((error) => {
          console.log(error);
        });
    },
    setCurrenRoom(room) {
      this.currentRoom = room;
      this.getAllMessages();
    },
    getAllMessages() {
      axios
        .get('/chat/room/'+this.currentRoom.id+'/messages')
        .then(response => {
          this.messages = response.data;
        })
        .catch(error => {
          console.log(error);
        });
    },
  },
  created(){
      this.getAllRooms()
      this.getAllMessages()
  }
});
</script>
