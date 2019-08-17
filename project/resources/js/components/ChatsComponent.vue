<template>
    <div class="row">
        <div class="col-sm-8">
            <div class="card card-default">
                <div class="card-header">
                    Messages
                </div>
                <div class="card-body p-0">
                    <ul class="list-unstyled" style="height: 300px; overflow-y: scroll" v-chat-scroll>
                        <li class="p-2" v-for="(message, index) in messages" :key="index" >
                            <strong>{{ message.user.name }}</strong>
                            {{ message.message }}
                        </li>
                    </ul>
                </div>

                <input type="text" 
                v-model="newMessage" 
                @keydown="sendTypingEvent"
                @keyup.enter="sendMessage" 
                name="message" placeholder="Enter your message..." 
                class="form-control">
                
            </div>
            <span class="text-muted" v-if="activeUser">{{ activeUser.name }} is typing...</span>
        </div> 
        <div class="col-sm-4">
            <div class="card card-default">
                <div class="card-header">Active Users</div>
                <div class="card-body">
                    <ul>
                        <li class="py-2" v-for="(user, index) in users" :key="index">
                            {{ user.name }}
                        </li>
                    </ul>
                </div>
            </div>
        </div>   
    </div>
</template>

<script>
import { setTimeout } from 'timers';

    export default {
        props: ['user'],
        data(){
            return {
                messages: [],
                newMessage: '',
                users: [],
                activeUser: false,
                typingTimer: false
            }
        },
        created() {
            this.fetchMessages();

            Echo.join('chat')
                .here(users => {
                    this.users = users;
                })
                .joining(user => {
                    this.users.push(user);
                })
                .leaving(user => {
                    this.users = this.users.filter(u => u.id != user.id);
                })
                .listen('MessageSent', (event) => {
                    this.messages.push(event.message)
                })
                .listenForWhisper('typing', user => {

                    this.activeUser = user;
                    if(this.typingTimer){
                        clearTimeout(this.typingTimer);
                    }
                    this.typingTimer = setTimeout(() => {
                        this.activeUser = false;
                    }, 3000);
                    
                })
        },
        methods: {
            fetchMessages(){
                axios.get('messages')
                    .then(response => {
                        this.messages = response.data;
                    })
            },
            sendMessage(){
                this.messages.push({
                    message: this.newMessage,
                    user: this.user
                });

                axios.post('messages', {message: this.newMessage})
                    .then(response => {
                        this.newMessage  = '';
                    })

                
            },
            sendTypingEvent(){
                Echo.join('chat')
                    .whisper('typing', this.user)
            }
        }
    }
</script>
