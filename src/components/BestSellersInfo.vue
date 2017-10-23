<template>
  <div class="container">
    <div class="box">
      <table class="table is-bordered is-striped is-narrow is-hoverable is-fullwidth">
        <thead>
          <tr>
            <th>Variation</th>
            <th>Test Goal</th>
            <th>Visitors</th>
            <th>Conversions</th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <th>CONTROL (A)</th>
            <td>Visited Best Sellers (CONTROL) & Converted</td>
            <td>{{ controlVisits[0].conversions }}</td>
            <td>{{ controlConversions[0].conversions }}</td>
          </tr>
          <tr>
            <th>Facebook Sorted (B)</th>
            <td>Visited Best Sellers (TEST) & Converted</td>
            <td>{{ variationBVisits[0].conversions }}</td>
            <td>{{ variationBConversions[0].conversions }}</td>
          </tr>
        </tbody>
      </table>
    </div>

    <div class="box">
      <h2>Control (<strong>Default</strong>)  &mdash; {{ conversionPercentageControl }}% Total Conversion</h2>
      <progress class="progress is-success" :value="conversionPercentageControl" max="100">{{ conversionPercentageControl }}%</progress>
      <h2>Variation B (<strong>Facebook Sorted</strong>) &mdash; {{ conversionPercentageB }}% Total Conversion</h2>
      <progress class="progress is-success" :value="conversionPercentageB" max="100">{{ conversionPercentageB }}%</progress>
    </div>
  </div>
</template>

<script>
import request from 'superagent';
import _ from 'lodash';

export default {
  name: 'BestSellersInfo',
  data () {
    return {
      responseData: [],
      conversionPercentageControl: 0,
      conversionPercentageB: 0,
    }
  },

  computed: {
    controlVisits() {
      return this.responseData.filter((row) => {
        return (row.variation_id == '9134741256' && row.goal_id == '9110790991');
      });
    },
    controlConversions() {
      return this.responseData.filter((row) => {
        return (row.variation_id == '9134741256' && row.goal_id == '9013618742');
      });
    },
    variationBVisits() {
      return this.responseData.filter((row) => {
        return (row.variation_id == '9143810282' && row.goal_id == '9103681845');
      });
    },
    variationBConversions() {
      return this.responseData.filter((row) => {
        return (row.variation_id == '9143810282' && row.goal_id == '9030286332');
      });
    },
  },

  watch: {
    controlVisits() {
      if (this.responseData) {
        this.conversionPercentageControl = ((this.controlConversions[0].conversions / this.controlVisits[0].conversions) * 100).toFixed(2);
      }
    },
    variationBVisits() {
      if (this.responseData) {
        this.conversionPercentageB = ((this.variationBConversions[0].conversions / this.variationBVisits[0].conversions) * 100).toFixed(2);
      }
    },
  },

  mounted() {
    var vm = this;
    var experimentNumber = '9117322852';

    request
      .get(`https://www.optimizelyapis.com/experiment/v1/experiments/${experimentNumber}/stats`)
      .set('Token', '17b1c96e0932b744a81f34b0ad8fdd6173f2e8c149ab01e21025b304fc9a4bd3:59O6z47zQ')
      .set('accept', 'json')
      .end((err, res) => {
        console.log(res.body);
        vm.responseData = res.body;
      });
  }
}
</script>

<style scoped>
.container {
  padding: 30px 0;
}
</style>
