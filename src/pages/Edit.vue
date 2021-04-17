<template>
  <div>
    <div class="row" v-if="asset">
      <div class="col-md-4">
        <img style="width: 100%" class="avatar" :src="asset.image" alt="...">
      </div>
      <div class="col-md-8">
        <div class="row">
          <div class="col-md-12">
            <base-input label="Organization"
                      placeholder="org"
                      v-model="asset.fullName"
                      disabled>
            </base-input>
          </div>
        </div>
        <div class="row">
          <div class="col-md-6">
            <base-input label="Username"
                      placeholder="Username"
                      v-model="asset.title"
            >
            </base-input>
          </div>
          <div class="col-md-6">
            <base-input label="Password"
                      placeholder="Username"
                      v-model="asset.password"
                      type="password"
            >
            </base-input>
          </div>
          <div class="container">
          <br>
          <br>
            <h4>Policy:</h4>
            <div class="container" style="margin-left: 40px">
              <div v-for="option in asset.options" :key="option.id" class="form-check row">
                <input class="form-check-input" type="radio" name="flexRadioDefault" :id="'option_'+option.id" :value="option.id" v-model="selected_policy">
                <label class="form-check-label col-md-4" :for="'option_'+option.id" >
                  {{option.text}}   
                </label>
                <select v-if="option.userSelect && selected_policy==option.id" class="form-select" aria-label="Default select example">
                  <option selected>Select a User</option>
                  <option v-for="person in users" :key="person.id" :value="person.id">{{person.name}}</option>
                </select>

              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
    <br>
    <br>
    <br>
    <div class="row container">
      <div class="col-md-2">
      <base-button slot="footer" type="button" @click="$router.push('/')" fill>Back</base-button>
      </div>
      <div class="col-md-2">
      <base-button slot="footer" type="button" @click="$router.push('/')" fill>Save</base-button>
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
        asset: null,
        selected_policy: 0,
        users: [
          {
            id:1,
            name: "Person 1"
          },
          {
            id:2,
            name: "Person 2"
          },
          {
            id:3,
            name: "Person 3"
          }
        ]
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
      console.log('lol')
      var all_assests = {
        user1: {
          id:1,
          fullName: 'www.facebook.com',
          image: 'img/facebook.png',
          title: 'Nikhil Jain',
          password: 'password',
          options: [
            {
              id: 1,
              text: "Choose legacy contact",
              userSelect: true
            },
            {
              id: 2,
              text: "Delete Account",
              userSelect: false
            },
            { 
              id: 3,
              text: "Memorialize Account",
              userSelect: false
            }
          ]
        },
        user2: {
          id:2,
          fullName: 'Metamask',
          image: 'img/metamask.svg',
          title: '0xa6b38D2094ce7ccF9306Fd7e7f7458',
          password: 'password',
          options: [
            {
              id: 1,
              text: "Transfer Savings",
              userSelect: true
            },
            {
              id: 2,
              text: "Delete Account",
              userSelect: false
            },
          ]
        },
        user3: {
          id:3,
          fullName: 'www.binance.com',
          image: 'img/binance.png',
          title: 'nikhil.jain@gmail.com',
          password: 'password',
          options: [
            {
              id: 1,
              text: "Transfer Savings",
              userSelect: true
            },
            {
              id: 2,
              text: "Choose Legacy Contact",
              userSelect: true
            },
            { 
              id: 3,
              text: "Delete Account",
              userSelect: false
            }
          ]
        },
        user4: {
          id:4,
          fullName: 'www.icloud.com',
          image: 'img/icloud.png',
          title: 'nikhil@icloud.com',
          password: 'password',
          options: [
            {
              id: 1,
              text: "Delete Private Data",
              userSelect: false
            },
            {
              id: 2,
              text: "Delete Account",
              userSelect: false
            },
            { 
              id: 3,
              text: "Legacy contact",
              userSelect: true
            }
          ]
        },
        user5: {
          id:5,
          fullName: 'accounts.google.com',
          image: 'img/google.png',
          title: 'nikhil@gmail.com',
          password: 'password',
          options: [
            {
              id: 1,
              text: "Delete Private Data",
              userSelect: false
            },
            {
              id: 2,
              text: "Delete Account",
              userSelect: false
            },
            { 
              id: 3,
              text: "Legacy contact",
              userSelect: true
            }
          ]
        }
      }
      this.asset = Object.values(all_assests).find(asset => asset.id == this.$route.params.id)
      console.log(this.asset)
      console.log('lol')
      console.log('lol')
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
