<template>
  <div class="hello">
    <h1> {{(format(time, 'hh:MM:ss A'))}} </h1>
    
    <button class="button-primary">Button element</button>
    
  </div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator';
import { format } from 'date-fns';

@Component
export default class HelloWorld extends Vue {
  
  time = new Date();
  
  @Prop() private msg!: string;

  mounted() {
    setInterval(() => {
      this.time = new Date()
    }, 1000);
  }
  format = format;
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
