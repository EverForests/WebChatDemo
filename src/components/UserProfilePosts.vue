<template>
    <div class="card">
        <div class="card-body">
            <div v-for="post in posts.posts" :key="post.id">
                <div class="card single-post">
                    <div class="card-body">
                        {{ post.content }}
                        <button v-if="is_me" @click="delete_a_post(post.id)" type="button" class="btn btn-danger btn-sm">删除</button>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import $ from 'jquery';
import { useStore } from 'vuex';

export default {
    name: 'UserProfilePosts',
    props: {
        is_me: {
            type: Boolean,
            required: true,
        },

        posts: {
            type: Object,
            required: true,
        }
    },
    setup(props, context) {
        const store = useStore();
        const delete_a_post = post_id => {
            $.ajax({
                url: "https://app165.acapp.acwing.com.cn/myspace/post/",
                type: "DELETE",
                data: {
                    post_id,
                },
                headers: {
                    'Authorization': "Bearer " + store.state.user.access,
                },
                success(resp) {
                    if (resp.result === "success") {
                        context.emit('delete_a_post', post_id);
                    }
                }
            })
        };

        return {
            delete_a_post
        }
    }
}
</script>

<style scoped>
.single-post {
    margin-bottom: 10px;
}

button {
    float: right;
}
</style>