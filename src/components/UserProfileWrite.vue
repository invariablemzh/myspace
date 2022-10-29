<template>
    <div class="card edit-up">
        <div class="card-body">
            <label for="edit-field" class="form-label">编辑帖子</label>
            <textarea v-model="content" class="form-control" id="edit-field" rows="3"></textarea>
            <button @click="post_a_post" type="button" class="btn btn-primary btn-sm">发帖</button>    
        </div>
    </div>
</template>


<script>
import { ref } from 'vue';
import $ from 'jquery';
import { useStore } from 'vuex'; 

export default{
    name: "UserProfileWrite",
    setup(props, context){
        const store = useStore();
        let content = ref('');

        const post_a_post = () => {
            $.ajax({
                url: "https://app165.acapp.acwing.com.cn/myspace/post/",
                type: "POST",
                data: {
                    content: content.value,
                },
                headers: {
                    'Authorization': "Bearer " + store.state.user.access,
                },
                success(resp) {
                    if(resp.result === "success"){
                        context.emit('post_a_post',content.value);
                        content.value = "";
                    }
                },

            });
        };
        return{
            content,
            post_a_post,
        }
    }
}

</script>



<style scoped>
.edit-up{
    margin-top: 20px;
}
</style>