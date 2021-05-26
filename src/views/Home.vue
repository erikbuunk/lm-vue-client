<template>
  <div class="home">
    <LogicMillSearch msg="Search Logic Mill" />
  </div>
</template>

<script>
// @ is an alias to /src
import LogicMillSearch from '@/components/LogicMillSearch.vue';

export default {
  name: 'Home',
  components: {
    // HelloWorld,
    LogicMillSearch,
  },
};
</script>
