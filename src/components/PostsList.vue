<template>
    <div class="list row">
        <div class="col-md-8">
            <div class="input-group mb-3">
                <input
                        type="text"
                        class="form-control"
                        placeholder="Search by title"
                        v-model="title"
                />
                <div class="input-group-append">
                    <button
                            class="btn btn-outline-secondary"
                            type="button"
                            @click="searchTitle"
                    >
                        Search
                    </button>
                </div>
            </div>
        </div>
        <div class="col-md-6">
            <h4>Posts List</h4>
            <ul class="list-group">
                <li
                        class="list-group-item"
                        :class="{ active: index === currentIndex }"
                        v-for="(post, index) in posts"
                        :key="index"
                        @click="setActiveTutorial(post, index)"
                >
                    {{ post.title }}
                </li>
            </ul>

            <button class="m-3 btn btn-sm btn-danger" @click="removeAllPosts">
                Remove All
            </button>
        </div>
        <div class="col-md-6">
            <div v-if="currentPost">
                <h4>Post</h4>
                <div>
                    <label><strong>Title:</strong></label> {{ currentPost.title }}
                </div>
                <div>
                    <label><strong>Content:</strong></label>
                    {{ currentPost.content }}
                </div>
                <div>
                    <label><strong>Status:</strong></label>
                    {{ currentPost.status ? "Published" : "Pending" }}
                </div>
                <div>
                    <label><strong>Create Date:</strong></label>
                    {{ currentPost.create_date }}
                </div>
                <div>
                    <label><strong>Modified Date:</strong></label>
                    {{ currentPost.modified_date }}
                </div>

                <a
                        class="badge badge-warning"
                        :href="'/posts/' + currentPost.id"
                >
                    Edit
                </a>
            </div>
            <div v-else>
                <br/>
                <p>Please click on a Post...</p>
            </div>
        </div>
    </div>
</template>

<script lang="ts">
import {Component, Vue} from "vue-property-decorator";
import PostDataService from "../services/PostDataService";

@Component
export default class PostsList extends Vue {
    private posts: any[] = [];
    private currentPost: any = null;
    private currentIndex: number = -1;
    private title: string = "";

    retrievePosts() {
        PostDataService.getAll()
            .then((response) => {
                this.posts = response.data;
                console.log(response.data);
            })
            .catch((e) => {
                console.log(e);
            });
    }

    refreshList() {
        this.retrievePosts();
        this.currentPost = null;
        this.currentIndex = -1;
    }

    setActiveTutorial(post: any, index: number) {
        this.currentPost = post;
        this.currentIndex = index;
    }

    removeAllPosts() {
        PostDataService.deleteAll()
            .then((response) => {
                console.log(response.data);
                this.refreshList();
            })
            .catch((e) => {
                console.log(e);
            });
    }

    searchTitle() {
        PostDataService.findByTitle(this.title)
            .then((response) => {
                this.posts = response.data;
                console.log(response.data);
            })
            .catch((e) => {
                console.log(e);
            });
    }

    mounted() {
        this.retrievePosts();
    }
}
</script>

<style scoped>
.list {
    text-align: left;
    max-width: 750px;
    margin: auto;
}
</style>