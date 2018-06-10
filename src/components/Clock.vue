<template>
  <div class="clock">
    <h1> {{(format(time, 'hh:MM:ss A'))}} </h1>
    
    <button class="button-primary">Button element</button>

    <div class="main">
      <div class="logs">
        <div class="time-log" v-for="t of intervalTimeLogs" :key="t.getTime()" >
          <div class="time-span">
            {{(format(t, 'hh:MM:ss A'))}}
          </div>
        </div>
      </div>
      <div class="messages">
        {{ stopwatch }}
      </div>
    </div>
    
  </div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator';
import { format, differenceInSeconds, differenceInMinutes, differenceInHours, differenceInMilliseconds } from 'date-fns';
import * as R from 'ramda';

@Component
export default class HelloWorld extends Vue {
  
  time = new Date();

  intervalSeconds = 105;
  stopwatch = '';

  intervalTimeLogs: Array<Date> = [];
  
  @Prop() private msg!: string;

  mounted() {

    this.time = new Date();
    this.intervalTimeLogs.push(new Date(this.time));

    setInterval(() => {
      this.time = new Date();
      const last: Date = this.intervalTimeLogs[this.intervalTimeLogs.length - 1];

      this.stopwatch = `${differenceInHours(this.time, last)}:${differenceInMinutes(this.time, last)}:${differenceInSeconds(this.time, last)}: ${differenceInMilliseconds(this.time, last)}`

      // this.stopwatch = format(Date.UTC(this.time - last), 'hh:MM:ss A');

      if (differenceInSeconds(this.time, last) >= this.intervalSeconds) {
        this.intervalTimeLogs.push(new Date(this.time));
      }

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

.main {
  height: calc(100vh - 210px);

  display: flex;
  flex-direction: row;

  .logs, .messages {
    flex: 1;
    border: 1px solid grey;
    height: 100%;
  }
}

</style>
