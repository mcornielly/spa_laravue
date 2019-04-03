<template>
        <div class="row">
            <div class="col-md-12">
                <div class="card mt-3 mb-3" v-for="item in lists">

                    <router-link 
                        class="card-header"
                        :to="{name: 'post', params: {slug: item.slug}}"
                        v-text="item.title"
                    ></router-link>

                    <div class="card-body">
                        <p>ID: {{ item.id }}</p>
                        <p class="card-text" v-text="item.excerpt"></p>
                    </div>
                </div>

                <infinite-loading @infinite="infiniteHandler">
                    <div slot="no-more"> -- </div>
                    <div slot="spinner"> Cargando... </div>
                    <div slot="no-results"> Sin resultados </div>
                </infinite-loading>
            </div>
        </div>
</template>

<script>
    export default {
        data() {
            return {
                lists: [],
                page: 0
            };
        },
        methods: {
            infiniteHandler($state) {
               this.page++
               let url = '/api/posts?page=' + this.page
               axios.get(url).then(response => {
                    let posts = response.data.data

                    if(posts.length){
                        this.lists = this.lists.concat(posts)
                        $state.loaded()
                    }else{
                        $state.complete()
                    }
               });
            }
        } 
    }
</script>
