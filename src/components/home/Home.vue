<template>
    <div class="home">
        <PageTitle icon="fa fa-home" main="Dashboard"
            sub="Base de conhecimento." />
            <div class="stats">
                <StatApp title="Categorias" :value="stats.categories"
                icon="fa fa-folder-open" color="#4dbd74" />

                <StatApp title="Artigos" :value="stats.articles"
                icon="fa fa-file-text" color="#f86c6b" />

                <StatApp title="Usuários" :value="stats.users"
                icon="fa fa-users" color="#36a3f7" />
            </div>
    </div>
  
</template>

<script>
import PageTitle from '../template/PageTitle';
import StatApp from './Stat'
import axios from 'axios'
import { baseApiUrl } from '@/global';
export default {
    name: 'HomeApp',
    components: { PageTitle, StatApp },
    data () {
        return {
            stats: [],
            
        }
    },
    methods: {
        getStats() {
            axios.get(`${baseApiUrl}/stats`)
                .then(res => {
                    this.stats = res.data
                })
        }
    },
    mounted() {
        this.getStats()
    }
    
}
</script>

<style>

.stats{
    display:flex;
    justify-content:space-between;
    flex-wrap:wrap;
}

</style>