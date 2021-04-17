<template>
  <div>
    <div class="row">
      <div class="col-md-3">
        <user-card :user="user1"></user-card>
      </div>
      <div class="col-md-3">
        <user-card :user="user2"></user-card>
      </div>
      <div class="col-md-3">
        <user-card :user="user3"></user-card>
      </div>
      <div class="col-md-3">
        <user-card :user="user4"></user-card>
      </div>
      <div class="col-md-3">
        <user-card :user="user5"></user-card>
      </div>
    </div>
  </div>
</template>
<script>
  import LineChart from '@/components/Charts/LineChart';
  import BarChart from '@/components/Charts/BarChart';
  import * as chartConfigs from '@/components/Charts/config';
  import TaskList from './Dashboard/TaskList';
  import UserTable from './Dashboard/UserTable';
  import config from '@/config';
  import UserCard from './Profile/UserCard'

  export default {
    components: {
      LineChart,
      BarChart,
      TaskList,
      UserTable,
      UserCard
    },
    data() {
      return {
        user1: {
          id:1,
          fullName: 'www.facebook.com',
          image: 'img/facebook.png',
          title: 'Nikhil Jain',
        },
        user2: {
          id:2,
          fullName: 'Metamask',
          image: 'img/metamask.svg',
          title: '0xa6b38D2094ce7ccF9306Fd7e7f7458',
        },
        user3: {
          id:3,
          fullName: 'www.binance.com',
          image: 'img/binance.png',
          title: 'nikhil.jain@gmail.com',
        },
        user4: {
          id:4,
          fullName: 'www.icloud.com',
          image: 'img/icloud.png',
          title: 'nikhil@icloud.com',
        },
        user5: {
          id:5,
          fullName: 'accounts.google.com',
          image: 'img/google.png',
          title: 'nikhil@gmail.com',
        },
      }
    },
    computed: {
      enableRTL() {
        return this.$route.query.enableRTL;
      },
      isRTL() {
        return this.$rtl.isRTL;
      },
      bigLineChartCategories() {
        return this.$t('dashboard.chartCategories');
      }
    },
    methods: {
      initBigChart(index) {
        let chartData = {
          datasets: [{
            fill: true,
            borderColor: config.colors.primary,
            borderWidth: 2,
            borderDash: [],
            borderDashOffset: 0.0,
            pointBackgroundColor: config.colors.primary,
            pointBorderColor: 'rgba(255,255,255,0)',
            pointHoverBackgroundColor: config.colors.primary,
            pointBorderWidth: 20,
            pointHoverRadius: 4,
            pointHoverBorderWidth: 15,
            pointRadius: 4,
            data: this.bigLineChart.allData[index]
          }],
          labels: ['JAN', 'FEB', 'MAR', 'APR', 'MAY', 'JUN', 'JUL', 'AUG', 'SEP', 'OCT', 'NOV', 'DEC'],
        }
        this.$refs.bigChart.updateGradients(chartData);
        this.bigLineChart.chartData = chartData;
        this.bigLineChart.activeIndex = index;
      }
    },
    mounted() {
      this.i18n = this.$i18n;
      if (this.enableRTL) {
        this.i18n.locale = 'ar';
        this.$rtl.enableRTL();
      }
      this.initBigChart(0);
    },
    beforeDestroy() {
      if (this.$rtl.isRTL) {
        this.i18n.locale = 'en';
        this.$rtl.disableRTL();
      }
    }
  };
</script>
<style>
</style>
