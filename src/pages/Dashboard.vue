<template>
  <div>
    <div class="container row">
      <div class="col-md-2"><label>Choose Heir   :    </label>
      </div>
      <v-select
      style="width:60%"
      class="style-chooser"
      placeholder="Choose a heir"
      :options="users"
      :value="heir" 
      label='name'
      @input="setSelected" 
    />
      <!-- :reduce="user => user.first_name + ' ' + user.last_name" -->
    </div>
    <br>
    <br>
    <div class="container row text-center" style="float:center">
      <base-button type="button" @click="deactivate" fill>Deactivate this account</base-button>
    </div>
    <br>
    <hr>
    <h2>Assets</h2>
    <div class="row">
      <div class="col-md-3" v-for="asset in assets" :key="asset.id">
        <user-card :user="asset"></user-card>
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
  import "vue-select/dist/vue-select.css";
  // import 

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
        assets:[],
        users:[],
        heir:null
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
      },
      setSelected(value){
        console.log(value)
        this.heir = value
        this.axios.patch('users/me',{heir_id: value.id},{headers:{'Authorization': localStorage.getItem('Authorization')}})
      },
      deactivate(){
        this.axios.get('/users/me',{headers:{'Authorization': localStorage.getItem('Authorization')}}).then(res2=>{
          this.axios.post('/users/'+res2.data.id+'/deactivate',{},{headers:{'Authorization': localStorage.getItem('Authorization')}}).then(res=>{
            this.$router.go()
            console.log(res)
          })
        })
      }
    },
    mounted() {
      const available = [{
          id:1,
          fullName: 'www.facebook.com',
          image: 'img/facebook.png',
          title: 'Nikhil Jain',
          has: true
        },
        {
          id:2,
          fullName: 'Metamask',
          image: 'img/metamask.svg',
          title: '0xa6b38D2094ce7ccF9306Fd7e7f7458',
          has: true
        },
        {
          id:3,
          fullName: 'www.binance.com',
          image: 'img/binance.png',
          title: 'nikhil.jain@gmail.com',
          has: true
        },
        {
          id:4,
          fullName: 'www.icloud.com',
          image: 'img/icloud.png',
          title: 'nikhil@icloud.com',
          has: true
        },
        {
          id:5,
          fullName: 'accounts.google.com',
          image: 'img/google.png',
          title: 'nikhil@gmail.com',
          has: true
        },
      ]
      console.log(localStorage.getItem('Authorization'))
      if (localStorage.getItem('Authorization') != undefined && localStorage.getItem('Authorization') != "undefined"){
        this.axios.get('/users/all').then(res=>{
          this.users = res.data.map(user=>{
            user.name = user.first_name + " " + user.last_name
            return user
          }).filter(user => user.email != localStorage.getItem('username'))
          console.log(localStorage.getItem('username'))
          console.log(this.users)
          this.axios.get('/users/me',{headers:{'Authorization': localStorage.getItem('Authorization')}}).then(res2=>{
            this.heir = this.users.find(user=>res2.data.heir_id == user.id)
          })
        })
        this.axios.get('assets',{headers:{'Authorization': localStorage.getItem('Authorization')}})
        .then((response) => {
          console.log(response.data)
          this.assets = available.map(asset => {
            var found = response.data.find(val => val.name == asset.fullName)
            if (found != undefined){
              asset.title = found.public_key

            }
            else{
              asset.has = false
            }
            // console.log(found)
            // asset.has = (found != undefined)
            return asset
          })
          this.assets = this.assets.filter(asset=>asset.has)
          console.log(this.assets)
      })
      }
      else{
        this.$router.push('/login')
      }
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
  .style-chooser .vs__search::placeholder,
  .style-chooser .vs__dropdown-toggle,
  .style-chooser .vs__dropdown-menu {
    background: #dfe5fb;
    border: none;
    color: #394066;
    text-transform: lowercase;
    font-variant: small-caps;
  }

  .style-chooser .vs__clear,
  .style-chooser .vs__open-indicator {
    fill: #394066;
  }
</style>
