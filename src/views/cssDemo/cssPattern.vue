<template>
  <div class="container-full css-pattern">
    <div class="box pink">
      <steamer />
    </div>
    <div class="box">aa</div>
    <div class="box">bb</div>
    <div class="box">vv</div>
  </div>
</template>
<script>
import steamer from './module/steamer'
export default {
  components: { steamer }
}
</script>
<style lang="scss" scoped>
.css-pattern{
  display: grid;
  grid-template-columns: repeat(3, 33.3%);
  grid-template-rows: repeat(3, 100%);
}
.box{
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 20px;
}
.pink{
  background-color: pink;
}
</style>
