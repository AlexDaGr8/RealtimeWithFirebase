<template>
    <div class="page">
        <div v-if="page">
            <label for="title">Title</label>
            <input type="text" name="title" v-model="page.title" class="title" placeholder="Enter Title" />
            <label for="content">Content</label>
            <textarea class="content" name="content" v-model="page.content" placeholder="Enter some content" @keyup="updateContent()" @keyup.enter="savePage()"></textarea>
            <button @click="deletePage()" class="red">Delete Page</button>
            <button 
                @click="savePage()"
                v-bind:class="{ 'blue': page['.key'] }"
                >
                <span v-if="page['.key']">Update</span>
                <span v-else>Save</span> 
                Page
            </button>
        </div>
        <div v-else> 
            <h1>&larr; To start, create a enw page!</h1>
        </div>
    </div>
</template>

<script>
export default {
    name: 'Page', 
    props: ['page'],
    methods: {
        deletePage() {
            this.$emit('delete-page', this.page)
        },
        savePage() {
            this.$emit('save-page', this.page)
        },
        updateContent() {
            this.$emit('update-page', this.page)
        }
    }
}
</script>

    <style scoped>
        .page {
            width: 100%;
            padding: 2rem;
            box-shadow: 3rem 0 5rem 3rem #c1f5ff;
        }

        .content, .title {
            border-style: none;
            border-radius: 0.25rem;
            border: solid 1px lightgray;
            width: 100%;
            box-sizing: border-box;
            margin-bottom: 1.25rem;
        }

        .content:focus, .title:focus {
            outline: 0;
        }

        .content {
            font-family: 'Avenir', Helvetica, Arial, sans-serif;
            resize: vertical;
            font-size: 1.5rem;
            padding: 0.5rem;
            height: 20rem;
        }

        .title {
            font-size: 2rem;
            padding: 0.5rem 1rem;
        }

        label {
            margin-bottom: 0.5rem;
            display: inline-block;
        }

        button {
            border-style: none;
            padding: 0.75rem 1rem;
            background-color: #6BF178;
            margin: 1rem;
            border-radius: 0.3rem;
            color: white;
            font-size: 1rem;
            font-weight: 500; 
            cursor: pointer;
            border: solid #777 2px;
            color: #00272B;
        }
        button.blue {
            background-color: #35A7FF;
        }
        button.red {
            background-color: #FF5964;
        }
        button.yellow {
            background-color: #FFE74C;
        }
        button:hover {
            filter: saturate(3);
        }
    </style>