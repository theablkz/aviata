<template>
  <div class="main">
   <header-component />

    <div class="grid content">

      <div class="grid-col-1_4 grid-container-gap">
        <rate-option :selects="rateOption" @resetChoice="clearRateOption()" />
        <avia-company :selects="airlines" @resetChoice="clearAirline()" />
      </div>
      <div class="grid-col-4_13 grid-container-gap">
        <tickets v-for="(item, index) in ticketsData" :key="index" :item="item" />


      </div>

    </div>
  </div>
</template>

<script>
import HeaderComponent from '~/components/headerComponent'
import RateOption from '~/components/filter/rateOption'
import AviaCompany from '~/components/filter/aviaCompany'
import Tickets from '~/components/tickets'
const TICKETSDATA = require("~/assets/results.json")
export default {
  components: {
    HeaderComponent,
    RateOption,
    AviaCompany,
    Tickets
  },
  data: () => ({
    rateOption: [
      {
        name: "Только прямые",
        state: false,
        code: "directFlight"
      },
      {
        name: "Только с багажом",
        state: false,
        code: "service",
      },
      {
        name: "Только возвратные",
        state: false,
        code: "refundable"
      }
    ],
    airlines: [{"name":"Air Astana","state":false,"code":"KC"},{"name":"Uzbekistan Airways","state":false,"code":"HY"},{"name":"Emirates","state":false,"code":"EK"},{"name":"HR","state":false,"code":"HR"},{"name":"Flydubai","state":false,"code":"FZ"},{"name":"S7 Airlines","state":false,"code":"S7"},{"name":"Lufthansa","state":false,"code":"LH"},{"name":"Air Baltic","state":false,"code":"BT"},{"name":"China Southern Airlines","state":false,"code":"CZ"},{"name":"Aeroflot","state":false,"code":"SU"},{"name":"Belavia","state":false,"code":"B2"},{"name":"SCAT Airlines","state":false,"code":"DV"},{"name":"Turkish Airlines","state":false,"code":"TK"}],
  }),
  computed: {
    airlinesOb(){
      return Object.keys(this.airlines).map(keys => ({
        name: this.airlines[keys],
        state: false,
        code: keys
      }))
    },
    ticketsData(){
      return TICKETSDATA.flights
    }
  },
  methods: {
    clearRateOption(){
      this.rateOption.map(item => {item.state = false})
    },
    clearAirline(){
      this.airlines.map(item => {item.state = false})
    }
  }
}
</script>

<style lang="scss" scoped>
@import '~/assets/style/_mixins/values.scss';
.main{
  .grid-container-gap{
    display: grid;
    grid-row-gap: 1.2rem;
    height: max-content;
  }
  .content{
    margin-top: 2rem;
  }
}
</style>
