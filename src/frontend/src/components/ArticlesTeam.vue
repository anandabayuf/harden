<template>
    <div id="container">
        <v-list 
            dense
            class="container"
            width="auto"
        >
            <h1 id="title" class="h1 font-weight-light"
            :style="$vuetify.breakpoint.lg ? 'font-size:3vw' : 'font-size:5vw'">{{team_name}}</h1>
            <v-list-item
                v-for="(article, id) in articles_team"
                :key="id"
                ripple
                @click="onArticleClick(article.id_article, article.title)"
                class="tile"
                dense
                style="margin-bottom:1%"
            >
                <v-img
                    :src="article.thumbnail"
                        style="margin-right:1%"
                    :max-width="$vuetify.breakpoint.lg ? '30%' : '40%'">
                </v-img>
                <v-list-item-content>
                    <h1
                        v-text="article.title"
                        class="h4 font-weight-light"
                        :style="$vuetify.breakpoint.lg ? 'font-size:1.5vw' : 'font-size:2.5vw'">
                    </h1>
                </v-list-item-content>
            </v-list-item>
        </v-list>
    </div>
</template>

<script>
import http from "@/http"

export default {
    name:'ArticleTeam',
    props:['id_team'],
    data(){
        return{
            articles_team:[],
            team_name:null,
            images:[]
        }
    },
    methods:{
        onArticleClick(id, title) {
      title = title.replace(/-|;|,|:|'|"|’|‘|“|”/g, '');
      title = title.replace(/\s+/g, '-');
      if (this.$route.path !== "/article/" + id + "/" + title) {
        this.$router.push("/article/" + id + "/" + title);
        this.$router.go();
      }
    }
    },
    mounted() {
        http.get('/articles/teams/' + this.id_team)
        .then((response) => {
            this.articles_team = response['data']
        })

        http.get('/teams/' + this.id_team)
        .then((response) => {
            const team = response['data']
            this.team_name = team[0].name
        })

        http.get('/images')
        .then((response) => {
            this.images = response['data']
        })
    }
}
</script>

<style scoped>
#container, .container, .tile{
  background: #222831;
}
.tile:hover {
  background: #203E5F;
}

h1, .display-2{
  color: #FFFFFF;
}
</style>