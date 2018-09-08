<docs>
Paydays Component
This component displays dates returned from a laravel api endpoint that calculates
the days on which I get paid.
</docs>

<template>
    <div class="box">
      <h2>Pay Days</h2>
        <ul v-for="payday in paydays" :key="payday">
            <li> {{ payday }} </li>
        </ul>
    </div>
</template>
<script>
/* @flow */
import axios from 'axios'
export default {
  name: 'paydays',
  data() {
    return {
      paydays: []
    }
  },
  mounted() {
    axios
      .get('/api/paydays')
      .then(response => {
        this.paydays = response.data
      })
      .catch(error => {
        console.log(error)
      })
  }
}
</script>
<style lang="">
</style>
