<template>
    <div class="submit-form">
        <div v-if="!submitted">
            <div class="form-group">
                <label for="title">Title</label>
                <input
                        type="text"
                        class="form-control"
                        id="title"
                        required
                        v-model="post.title"
                        name="title"
                />
            </div>

            <div class="form-group">
                <label for="content">Content</label>
                <textarea
                        class="form-control"
                        id="content"
                        required
                        v-model="post.content"
                        name="content"
                        rows="5"
                />
            </div>

            <button @click="savePost" class="btn btn-success">Submit</button>
        </div>

        <div v-else>
            <h4>You submitted successfully!</h4>
            <button class="btn btn-success" @click="newPost">Add</button>
        </div>
    </div>
</template>

<script lang="ts">
import {Component, Vue} from "vue-property-decorator";
import PostDataService from "../services/PostDataService";

@Component
export default class AddPost extends Vue {
    private post: any = {
        id: null,
        title: "",
        content: "",
        published: false,
    };

    private submitted: boolean = false;

    savePost() {
        var data = {
            title: this.post.title,
            content: this.post.content,
        };

        PostDataService.create(data)
            .then((response) => {
                this.post.id = response.data.id;
                console.log(response.data);
                this.submitted = true;
            })
            .catch((e) => {
                console.log(e);
            });
    }

    newPost() {
        this.submitted = false;
        this.post = {};
    }
}
</script>

<style scoped>
.submit-form {
    max-width: 300px;
    margin: auto;
}
</style>