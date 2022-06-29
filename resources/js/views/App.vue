<template>

    <div>
        <banner-component></banner-component>
        <section class="posts py-5">
            <div class="container">
                <div class="row row-cols-1 row-cols-sm-2 row-cols-md-3 gy-3">
                    <div class="col" v-for="post in postsResponse.data" :key="post.id">
                        <div class="product card">
                            <img height="250" :src="'storage/' + post.cover_image" :alt="post.title">
                            <div class="card-body">
                                <h5><strong>{{ post.title }}</strong></h5>
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
            <div class="text-center py-4">
                <nav aria-label="Page navigation">
                  <ul class="pagination justify-content-center">
                    <li class="page-item" v-if="postsResponse.current_page > 1">
                      <a class="page-link" href="#" aria-label="Previous" @click.prevent="getPosts(postsResponse.current_page - 1)">
                        <span aria-hidden="true">&laquo;</span>
                        <span class="sr-only">Previous</span>
                      </a>
                    </li>

                    <li :class="{ 'page-item': true, 'active': page == postsResponse.current_page }" v-for="page in postsResponse.last_page" :key="page.id">
                        <a class="page-link" href="#" @click.prevent="getPosts(page)">{{page}}</a>
                    </li>
                    

                    <li class="page-item" v-if="postsResponse.current_page < postsResponse.last_page">
                      <a class="page-link" href="#" aria-label="Next" @click.prevent="getPosts(postsResponse.current_page + 1)">
                        <span aria-hidden="true">&raquo;</span>
                        <span class="sr-only">Next</span>
                      </a>
                    </li>
                  </ul>
                </nav>
            </div>
            </div>
        </section>
    </div>
    
    

</template>


<script>
import BannerComponent from '../components/BannerComponent';
export default {
    name: 'App',
    components: { BannerComponent },
    data() {
        return {
          
            postsResponse: ''
        }
    },
    methods: {
        getPosts(postPage) {
            axios.get('/api/posts', {
                params: {
                    page: postPage
                }
            }).then((response) => {
            //console.log(response);
           
            this.postsResponse = response.data
        }).catch(e => {
            console.error(e);
        })
        }
    },
    mounted() {
        //console.log('mounted');
        this.getPosts(1)
    }
}
</script>

<style lang="scss" scoped>

.posts{
    .card-body {
        h5 {
            height: 50px;
        }
        p {
            height: 60px;
            overflow-y: auto;
            &::-webkit-scrollbar {
                display: none;
            }
        }
    }
}


</style>