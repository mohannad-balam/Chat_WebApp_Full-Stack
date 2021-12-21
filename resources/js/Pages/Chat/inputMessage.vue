<template>
    <div class="relative h-10 m1">
        <div style="border-top: 1px solid #e6e6e6" class="grid grid-cols-6">
            <input
            type="text"
            v-model="message"
            @keyup.enter="sendMessage()"
            placeholder = "type a message ..."
            class="col-span-5 outline-none p-1 rounded"
            />
            <button
            class="place-self-end bg-gray-500 hover:bg-green-700 p-1 m-1 rounded text-white w-full"
            @click="sendMessage()"
            >
            Send
            </button>
        </div>
    </div>
</template>

<script>
export default {
    props : ['room'],
    data : function(){
        return {
            message : '',
        }
    },
    methods : {
        sendMessage(){
            if(this.message == ' '){
                return;
            }

            axios.post('chat/room/'+this.room.id+'/message', {
                message : this.message
            })
            .then(response => {
                if(response.status == 201){
                    this.message = ''
                    this.$emit('messageSent')
                }
            })
            .catch(error => {
                console.log(error)
            })
        }
    }
}
</script>
