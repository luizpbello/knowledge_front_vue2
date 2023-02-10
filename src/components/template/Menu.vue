<template>
  <aside class="menu" v-show="isMenuVisible">
  <div class="menu-filter">
      <i class="fas fa-search" fa-lg></i>
        <input type="text" placeholder="Buscar" 
        v-model="treeFilter" class="filter-field">
  </div>
    <Tree :data="treeData" :options="treeOptions"
        :filter="treeFilter" ref="tree"
    />
  </aside>
</template>

<script>
import { mapState } from 'vuex'
import Tree from 'liquor-tree'
import { baseApiUrl } from '@/global'
import axios from 'axios'

export default {
    name: 'MenuApp',
    components: { Tree },
    computed: mapState(['isMenuVisible']),
    data(){
        return {
            treeFilter: '',
                treeData: this.getTreeData(),
                treeOptions: {
                    propertyNames: {'text': 'name' },
                    filter: { emptyText: ' Nenhum resultado encontrado'}
                }

            }
    },
    methods: {
        getTreeData() {
            const url = `${baseApiUrl}/categories/tree`
            return axios.get(url).then(res => res.data)
            
        },
        onNodeSelect(node){
            this.$router.push({ 
                name: 'articleByCategory',
                params: { id: node.id }
                
                })

                if(this.$mq == 'xs' || this.$mq =='sm'){
                    this.$store.commit('toggleMenu', false)
                }
        },


    },
    mounted() {
        this.$refs.tree.$on('node:selected', this.onNodeSelect)
    }
}
</script>

<style>
    .menu {
        grid-area: menu;
        background: linear-gradient(to right, #232526, #414345);

        display: flex;
        flex-direction: column;
        flex-wrap: wrap;
        
    }

    .menu a,
    .menu a:hover {
        color: #FFF;
        text-decoration: none;
    }

    .menu .tree-node.selected > .tree-content,
    .menu .tree-node .tree-content:hover  {
        background: #414345;
        color: #FFF;
    }

    .tree-arrow.has-child{
        filter:brightness(5)
    }

    .menu .menu-filter{
        display:flex;
        justify-content:center;
        align-items:center;

        margin:20px;
        margin-bottom: 8px;
        border-bottom: 1px solid #AAA;
    }

    .menu .menu-filter i{
        margin-right: 10px;
        color: #AAA;
    }

    .menu input {
        color: #CCC;
        font-size: 1rem;
        border: 0;
        outline:none;
        background: transparent;
        width: 100%;
    }

    .tree-filter-empty{
        color: #CCC;
        font-size: 1rem;
        margin-left: 20px;

    }
</style>