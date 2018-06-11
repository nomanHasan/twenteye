<template>
  <div class="clock">
    <h1> {{(format(time, 'hh:MM:ss A'))}} </h1>
    <h2> {{stopwatch}} </h2>
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
import { format, differenceInSeconds, differenceInMinutes, addMinutes, addHours, addSeconds, differenceInHours, differenceInMilliseconds, addDays, addMilliseconds } from 'date-fns';
import * as R from 'ramda';

@Component
export default class HelloWorld extends Vue {
  
  time = new Date();

  intervalSeconds = 5;
  stopwatch = '';

  intervalTimeLogs: Array<Date> = [];
  
  @Prop() private msg!: string;

  mounted() {

    this.time = new Date();
    this.intervalTimeLogs.push(new Date(this.time));

    setInterval(() => {
      this.time = new Date();
      const last: Date = this.intervalTimeLogs[this.intervalTimeLogs.length - 1];

      this.stopwatch = this.timeFormatter(this.calculateTimeDifference(this.time, last));

      // this.stopwatch = format(Date.UTC(this.time - last), 'hh:MM:ss A');

      if (differenceInSeconds(this.time, last) >= this.intervalSeconds) {
        this.intervalTimeLogs.push(new Date(this.time));
      }

    }, 13);
  }
  format = format;
  addSeconds = addSeconds;
  addMinutes = addMinutes;
  addHours = addHours;
  R = R;

  calculateTimeDifference = (time1: Date, time2: Date) => {
    const time = time1.getTime() - time2.getTime();

    let days, hours, minutes, seconds, millisseconds;

    const millUnit = 1;
    const secondUnit = 1000;
    const minuteUnit = secondUnit * 60;
    const hourUnit = minuteUnit * 60;
    const dayUnit = hourUnit * 24;

    days = time > dayUnit ? Math.floor(time / dayUnit) : 0;

    hours = time > hourUnit ? Math.floor((time % dayUnit) / hourUnit) : 0;

    minutes = time > minuteUnit ? Math.floor((time % hourUnit) / minuteUnit) : 0;
    
    seconds = time > secondUnit ? Math.floor((time % minuteUnit) / secondUnit) : 0;
    
    millisseconds = time > millUnit ? Math.floor((time % secondUnit)) : 0;

    return {
      days, hours, minutes, seconds, millisseconds
    };
  }

  timeFormatter = ({
      days = 0, hours = 0, minutes = 0, seconds = 0, millisseconds = 0
    }) => {
      let d, h, m, s, mi;
      d = days > 0 ? `${days}D ` : '';

      h = hours < 10 ? `0${hours}:` : `${hours}:`;
      m = minutes < 10 ? `0${minutes}:` : `${minutes}:`;
      s = seconds < 10 ? `0${seconds}:` : `${seconds}:`;
      mi = millisseconds < 10 ? `00${millisseconds}` : ( millisseconds < 100 ? `0${millisseconds}`: `${millisseconds}`);


      return d + h + m + s + mi;

    }


  demo = this.timeFormatter(this.calculateTimeDifference(addMilliseconds(addDays(addHours(addSeconds(addMinutes(new Date(), 21), 2), 22), 23), 19), new Date()));
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
