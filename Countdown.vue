<docs>
Countdown Component
This component accepts a human readable "deadline" prop and displays a countdown to this date.
I original created this to display a countdown to my 80th birthday, which coinsides with expected
lifespan of the average American male.
</docs>

<template>
    <div class="card">
        <header class="card-header">
            <p class="card-header-title">Time Remaining :</p>
        </header>
        <div class="card-content">
            <div class="content">
                <ul class="vue-countdown">
                    <li class="unit">
                        <p class="digit">{{ days | twoDigits }}</p>
                        <p class="text">days</p>
                    </li>
                    <li class="unit">
                        <p class="digit">{{ hours | twoDigits }}</p>
                        <p class="text">hours</p>
                    </li>
                    <li class="unit">
                        <p class="digit">{{ minutes | twoDigits }}</p>
                        <p class="text">Min</p>
                    </li>
                    <li class="unit">
                        <p class="digit">{{ seconds | twoDigits }}</p>
                        <p class="text">Sec</p>
                    </li>
                </ul>
            </div>
        </div>
        <footer class="card-footer"></footer>
    </div>
</template>
<script>
/* @flow */
export default {
  name: 'countdown',
  props: ['deadline'],
  data() {
    return {
      now: Math.trunc(new Date().getTime() / 1000),
      date: null
    }
  },
  mounted() {
    this.date = Math.trunc(Date.parse(this.deadline) / 1000)
    setInterval(() => {
      this.now = Math.trunc(new Date().getTime() / 1000)
    }, 1000)
  },
  computed: {
    seconds() {
      return Math.trunc(this.date - this.now) % 60
    },
    minutes() {
      return Math.trunc((this.date - this.now) / 60) % 60
    },
    hours() {
      return Math.trunc((this.date - this.now) / 60 / 60) % 24
    },
    days() {
      return Math.trunc((this.date - this.now) / 60 / 60 / 24)
    }
  },
  filters: {
    twoDigits: value => {
      if (value.toString().length <= 1) {
        return '0' + value.toString()
      }
      return value.toString()
    }
  }
}
</script>
<style lang="scss">
.vue-countdown {
  padding: 0;
  margin: 0;
}

.unit {
  display: inline-block;
  margin: 0 8px;
  text-align: center;
  position: relative;
}

.digit {
  font-size: 32px;
  font-weight: 600;
  line-height: 1.4;
  margin-bottom: 0;
}

.text {
  text-transform: uppercase;
  margin-bottom: 0;
  font-size: 10px;
}
</style>
