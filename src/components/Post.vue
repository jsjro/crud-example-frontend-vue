<template>
    <div v-if="currentPost" class="edit-form">
        <h4>Post</h4>
        <form>
            <div class="form-group">
                <label for="title">Title</label>
                <input
                        type="text"
                        class="form-control"
                        id="title"
                        v-model="currentPost.title"
                />
            </div>
            <div class="form-group">
                <label for="content">Content</label>
                <input
                        type="text"
                        class="form-control"
                        id="content"
                        v-model="currentPost.content"
                />
            </div>

            <div class="form-group">
                <label><strong>Status:</strong></label>
                {{ currentPost.status ? "Published" : "Pending" }}
            </div>
        </form>

        <button
                class="badge badge-primary mr-2"
                v-if="currentPost.status"
                @click="updateStatus(false)"
        >
            UnPublish
        </button>
        <button
                v-else
                class="badge badge-primary mr-2"
                @click="updateStatus(true)"
        >
            Publish
        </button>

        <button class="badge badge-danger mr-2" @click="deletePost">
            Delete
        </button>

        <button type="submit" class="badge badge-success" @click="updatePost">
            Update
        </button>
        <p>{{ message }}</p>
    </div>

    <div v-else>
        <br/>
        <p>Please click on a Post...</p>
    </div>
</template>

<script lang="ts">
import {Component, Vue} from "vue-property-decorator";
import PostDataService from "../services/PostDataService";

@Component
export default class Post extends Vue {
    private currentPost: any = null;
    private message: string = "";

    getPost(id: string) {
        PostDataService.get(id)
            .then((response) => {
                this.currentPost = response.data;
                console.log(response.data);
            })
            .catch((e) => {
                console.log(e);
            });
    }

    updateStatus(status: boolean) {
        var data = {
            id: this.currentPost.id,
            title: this.currentPost.title,
            content: this.currentPost.content,
            create_date: this.currentPost.create_date,
            modified_date: this.currentPost.modified_date,
            status: status,
        };

        PostDataService.update(this.currentPost.id, data)
            .then((response) => {
                this.currentPost.status = status;
                console.log(response.data);
            })
            .catch((e) => {
                console.log(e);
            });
    }

    updatePost() {
        PostDataService.update(this.currentPost.id, this.currentPost)
            .then((response) => {
                console.log(response.data);
                this.message = "The tutorial was updated successfully!";
            })
            .catch((e) => {
                console.log(e);
            });
    }

    deletePost() {
        PostDataService.delete(this.currentPost.id)
            .then((response) => {
                console.log(response.data);
                this.$router.push({name: "posts"});
            })
            .catch((e) => {
                console.log(e);
            });
    }

    mounted() {
        this.message = "";
        this.getPost(this.$route.params.id);
    }
}
</script>

<style scoped>
.edit-form {
    max-width: 300px;
    margin: auto;
}
</style>