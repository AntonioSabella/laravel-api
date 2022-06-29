<template>

    <div>
        <work-in-progress></work-in-progress>
        <section class="posts">
            <div class="container">
                <div class="row row-cols-1 row-cols-sm-2 row-cols-md-3">
                    <div class="col" v-for="post in postsResponse.data" :key="post.id">
                        <div class="product card">
                            <img :src="'storage/' + post.cover_image" :alt="post.title">
                            <div class="card-body">
                                <h3><strong>{{ post.title }}</strong></h3>
                                <p><strong>{{ post.content }}</strong></p>
                            </div>
                            <div class="card-footer">
                                <div class="row">
                                    <div class="col">
                                        <div class="author">
                                            <h4>Author:</h4>
                                            <p> L'autore al momento non c'è</p>
                                        </div>
                                    </div>
                                    <div class="col">
                                         <span v-if="post.category"><strong>Category:</strong>{{post.category.name}}</span>
                                         <div class="tags" v-if="post.tags.length > 0">
                                           <strong>Tags:</strong>
                            
                                          <span v-for="tag in post.tags" :key="tag.id">
                                          #{{tag.name}}
                                          </span>
                                     </div>
                                    </div>
                                </div>
                               
                               
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </section>
    </div>
    
    

</template>


<script>
import WorkInProgress from '../components/WorkInProgress';
export default {
    name: 'App',
    components: { WorkInProgress },
    data() {
        return {
            posts:'',
            postsResponse: ''
        }
    },
    methods: {
        getAllPosts() {
            axios.get('/api/posts').then((response) => {
            //console.log(response);
            this.posts = response.data.data
            this.postsResponse = response.data
        }).catch(e => {
            console.error(e);
        })
        }
    },
    mounted() {
        //console.log('mounted');
        this.getAllPosts()
    }
}
</script>