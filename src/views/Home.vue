<template>
  <div class="home">
    <Clock></Clock>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
import Clock from '@/components/Clock.vue'; // @ is an alias to /src

@Component({
  components: {
    Clock,
  },
})
export default class Home extends Vue {}
</script>
