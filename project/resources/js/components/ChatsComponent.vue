<template>
    <div class="row">
        <div class="col-sm-8">
            <div class="card card-default">
                <div class="card-header">
                    Messages
                </div>
                <div class="card-body p-0">
                    <ul class="list-unstyled" style="height: 300px; overflow-y: scroll">
                        <li class="p-2">
                            <strong>Shibnath</strong>
                            Message text
                        </li>
                    </ul>
                </div>

                <input type="text" name="message" placeholder="Enter your message..." class="form-control">
                
            </div>
            <span class="text-muted">User is typing...</span>
        </div> 
        <div class="col-sm-4">
            <div class="card card-default">
                <div class="card-header">Active Users</div>
                <div class="card-body">
                    <ul>
                        <li class="py-2">
                            Shibnath
                        </li>
                    </ul>
                </div>
            </div>
        </div>   
    </div>
</template>

<script>
    export default {
        mounted() {
            console.log('Component mounted.')
        }
    }
</script>
