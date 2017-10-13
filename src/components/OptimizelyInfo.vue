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
          <tr v-for="item in controlData" :key="item.variation_id + item.goal_id">
            <th>{{item.variation_name}}</th>
            <td>{{item.goal_name}}</td>
            <td>{{item.visitors}}</td>
            <td>{{item.conversions}}</td>
          </tr>
          <tr v-for="item in variationDataA" :key="item.variation_id + item.goal_id">
            <th>{{item.variation_name}}</th>
            <td>{{item.goal_name}}</td>
            <td>{{item.visitors}}</td>
            <td>{{item.conversions}}</td>
          </tr>
          <tr v-for="item in variationDataB" :key="item.variation_id + item.goal_id">
            <th>{{item.variation_name}}</th>
            <td>{{item.goal_name}}</td>
            <td>{{item.visitors}}</td>
            <td>{{item.conversions}}</td>
          </tr>
        </tbody>
      </table>
    </div>

    <div class="box">
      <h2>Control &mdash; {{ controlConversionPercentage }}% Total Conversion</h2>
      <progress class="progress is-success" :value="controlConversionPercentage" max="100">{{ controlConversionPercentage }}%</progress>
      <h2>Variation A (<strong>10% Off</strong>) &mdash; {{ conversionPercentageA }}% Total Conversion</h2>
      <progress class="progress is-success" :value="conversionPercentageA" max="100">{{ conversionPercentageA }}%</progress>
      <h2>Variation B (<strong>$25 Insurance</strong>) &mdash; {{ conversionPercentageB }}% Total Conversion</h2>
      <progress class="progress is-success" :value="conversionPercentageB" max="100">{{ conversionPercentageB }}%</progress>
      <hr/>
      <h2>Variation A &mdash; {{ optInPercentageA }}% Opted-IN for <strong>10% Off</strong></h2>
      <progress class="progress is-info" :value="optInPercentageA" max="100">{{ optInPercentageA }}%</progress>
      <h2>Variation B &mdash; {{ optInPercentageB }}% Opted-IN for <strong>$25 Insurance</strong></h2>
      <progress class="progress is-info" :value="optInPercentageB" max="100">{{ optInPercentageB }}%</progress>
    </div>
  </div>
</template>

<script>
import request from 'superagent';
import _ from 'lodash';

export default {
  name: 'OptimizelyInfo',
  data () {
    return {
      responseData: [],
      optInPercentageA: 0,
      optInPercentageB: 0,
      conversionPercentageA: 0,
      conversionPercentageB: 0,
      controlConversionPercentage: 0,
    }
  },

  computed: {
    controlData() {
      return this.responseData.filter((row) => {
        return row.variation_id == '8904191412' && row.goal_id == '8897983017';
      });
    },
    variationDataA() {
      return this.responseData.filter((row) => {
        return (row.variation_id == '8907070083' && row.goal_id == '8893093256') || (row.variation_id == '8907070083' && row.goal_id == '8901852347');
      });
    },
    variationDataB() {
      return this.responseData.filter((row) => {
        return (row.variation_id == '8902140446' && row.goal_id == '8902182648') || (row.variation_id == '8902140446' && row.goal_id == '8895810948');
      });
    },
  },

  watch: {
    controlData() {
      if (this.controlData) {
        let totalConversions = this.controlData[0]['conversions'];
        let visitors = this.controlData[0]['visitors'];

        this.controlConversionPercentage = ((totalConversions/visitors) * 100).toFixed(2);
      }
    },
    variationDataA() {
      if (this.variationDataA) {
        let totalConversions = this.variationDataA.reduce((acc, curr) => acc.conversions + curr.conversions);
        let visitors = this.variationDataA[0]['visitors'];
        let optIn = this.variationDataA.filter(i => i.goal_id === 8901852347)[0]['conversions'];

        this.optInPercentageA = ((optIn/totalConversions) * 100).toFixed(2);
        this.conversionPercentageA = ((totalConversions/visitors) * 100).toFixed(2);
      }
    },
    variationDataB() {
      if (this.variationDataB) {
        let totalConversions = this.variationDataB.reduce((acc, curr) => acc.conversions + curr.conversions);
        let visitors = this.variationDataB[0]['visitors'];
        let optIn = this.variationDataB.filter(i => i.goal_id === 8895810948)[0]['conversions'];

        this.optInPercentageB = ((optIn/totalConversions) * 100).toFixed(2);
        this.conversionPercentageB = ((totalConversions/visitors) * 100).toFixed(2);
      }
    },
  },

  mounted() {
    var vm = this;

    request
      .get('https://www.optimizelyapis.com/experiment/v1/experiments/8909431206/stats')
      .set('Token', '17b1c96e0932b744a81f34b0ad8fdd6173f2e8c149ab01e21025b304fc9a4bd3:59O6z47zQ')
      .set('accept', 'json')
      .end((err, res) => {
        vm.responseData = res.body;
      });
  }
}
</script>
