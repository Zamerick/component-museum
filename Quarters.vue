<docs>
Quarters Component
Wrote this component for my dashboard project after reading "The 12 Week Year" by Brian Moran.
Displays the response from a laravel api endpoint that calculates the start and end dates of each week in a series of twelve weeks,
as well as spacer weeks of vacation/downtime between the 'years'(quarters)
</docs>

<template>
    <div class="box">
        <table v-for="(quarter, key) in quarters" :key="key" class="table is-bordered is-striped">
            <thead>
                <tr>
                    <th></th>
                    <th>Start</th>
                    <th>End</th>
                </tr>
            </thead>

            <tbody>
                <tr v-for="(value, key) in quarter" :key="key">
                    <th>{{key + 1}}</th>
                    <td> {{ value.start_date }} </td>
                    <td> {{ value.end_date }} </td>
                </tr>
            </tbody>
        </table>
    </div>
</template>
<script>
/* @flow */
import axios from 'axios'
export default {
  name: 'quarters',
  data() {
    return {
      quarters: []
    }
  },
  mounted() {
    axios
      .get('/api/quarters')
      .then(response => {
        this.quarters = response.data
      })
      .catch(error => {
        console.log(error)
      })
  }
}
</script>
<style lang="">
</style>
