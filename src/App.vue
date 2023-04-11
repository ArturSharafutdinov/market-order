<template>
  <div class="container q-pa-md">
    <router-view/>
  </div>
</template>

<script>

export default {
  name: 'LayoutDefault',

  components: {},
};
</script>

<style scoped lang="scss">
.container {
  width: 100%;
  height: 100%;
}
</style>
