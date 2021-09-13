<template>
  <div>
    <main v-if="!loading">
      <DataTitle :text="title"  :dataDate="dataDate" />
      <DataBoxes :stats="status" />
      <CountrySelect @get-country="getCountryData" :countries="countries" />
      <button class="bg-green-700 text-white rounded p-3 mt-1 mb-2 focus:outline-none hover:bg-green-600" v-if="status.Country" @click="clearCountryData">
        Clear Country
      </button>
    </main>
    <main class="flex flex-col align-center justify-center text-center" v-else>
      <div class="text-gray-500 text-3xl mt-10 mb-6">
        Fetching Data
      </div>
      <img :src="loadingImage" class="w-24 m-auto" alt="">
    </main>
  </div>
</template>

<script>
// @ is an alias to /src
import DataTitle from '@/components/DataTitle'
import DataBoxes from '@/components/DataBoxes'
import CountrySelect from '@/components/CountrySelect'
export default {
  name: 'Home',
  components: {
    DataTitle,
    DataBoxes,
    CountrySelect
  },
  data(){
    return{
      loading: true,
      title: 'Global',
      dataDate:'',
      status:{},
      countries:[],
      loadingImage: 'https://raw.githubusercontent.com/bradtraversy/vue-covid-tracker/master/src/assets/hourglass.gif'
    }
  },
  methods:{
    async fetchCovidData(){
      const res = await fetch('https://api.covid19api.com/summary')
      const data = await res.json()
      return data
    },
    async getCountryData(country){
      this.status = country
      this.title = country.Country
    },
    async clearCountryData(){
      this.loading = true
      const data =  await this.fetchCovidData()
      this.title = 'Global'
      this.status = data.Global
      this.loading = false
    }
  },
  async created(){
    const data = await this.fetchCovidData()
    console.log(data)

    this.dataDate = data.Date
    this.status = data.Global
    this.countries = data.Countries
    this.loading = false
  }
}
</script>
