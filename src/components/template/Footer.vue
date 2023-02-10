<template>
  <footer class="footer">
      <span>Luiz Bello</span>
  </footer>
</template>

<script>
export default {
    name:'FooterApp'

}
</script>

<style>
.footer{ 
    grid-area:footer;
    background-color: orangered;
    color:#333;

    display: flex;
    justify-content: flex-end;
    align-self: center;
    padding-right:10px;
}

</style>