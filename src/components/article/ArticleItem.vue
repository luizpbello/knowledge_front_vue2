<template>
<div class="article-item">
    <router-link :to="{ name: 'articleById', params: { id: article.id }}">
        <div class="article-item-image d-none d-sm-block">
            <img v-if="article.imageUrl" 
            :src="article.imageUrl"
            height="150" width="150" alt="Article">
            <img v-else src="@/assets/article2.jpg"
            height="150" width="150" alt="Article">

        </div>
        <div class="article-item-info">
            <h2>{{ article.name }}</h2>
            <p>{{ article.description }}</p>
            <span class="article-item-author">
                <strong>Autor: </strong>{{ article.author }}
            </span>
            
        </div>
    </router-link>
</div>
  
</template>

<script>
export default {
    name:'ArticleItem',
    props:['article']

}
</script>

<style>

.article-item{
    border-radius: 8px;
    margin-bottom: 20px;
    padding: 20px;
    background-color:#FFF;
    border: 1px solid rgba(0,0,0,0.2);
    box-shadow: 0 1px 5px rgba(0, 0, 0, 0.15);
}

.article-item:hover{
    box-shadow: 0 1px 5px rgba(0, 0, 0, 1);
    transform: translateX(15px);
    transition:all 0.3s ease-in-out;

}

.article-item a{
    text-decoration: none;
    display:flex;
    align-items:flex-start;
    color:black;
}

.article-item a:hover{
    text-decoration: none;
    color:black;
}

.article-item-info h2 {
    font-size: 1.5rem;
}

.article-item-image{
    padding-right: 20px;
    margin-right: 20px;
    border-right: 1px solid #AAA;
}

.article-item-image img{
    border-radius:5px;
}

.article-item-info {
    display:flex;
    flex-direction:column;
    align-self: stretch;

}

.article-item-info p{
    flex: 1;
    color: #555;
    font-size: 1rem;
}

</style>