<template>
  <div class="main">
   <header-component />

    <div v-if="!filterMobileState" class="tickets-filter_mobile">
      <div class="new-search">
        <p>новый поиск</p>
      </div>
      <div @click="filterMobileClick" class="tickets-filter">
        <reset-choice :color="'white'" :notification="false"/>
        <p>фильтры</p>
      </div>
    </div>

    <div v-if="filterMobileState" class=" tickets-filter-popup">
      <rate-option :selects="rateOption" @resetChoice="clearRateOption()" />
      <avia-company :selects="airlines" @resetChoice="clearAirline()" />
      <div @click="filterMobileClick" class="tickets-filter-popup__close">
        <p>закрыть</p>
      </div>
    </div>

    <div class="grid content">

      <div class="grid-col-1_4 grid-container-gap tickets-filter">
        <rate-option :selects="rateOption" @resetChoice="clearRateOption()" />
        <avia-company :selects="airlines" @resetChoice="clearAirline()" />
      </div>
      <div class="grid-col-4_13 grid-container-gap tickets-box">
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
import ResetChoice from '~/components/icons/resetChoice'
const TICKETSDATA = require("~/assets/results.json")

export default {
  components: {
    HeaderComponent,
    RateOption,
    AviaCompany,
    Tickets,
    ResetChoice
  },
  data: () => ({
    filterMobileState: false,
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
    airlines: [],

  }),
  computed: {

    ticketsData(){
      TICKETSDATA.flights.map(item => {
        item.directFlight = item.itineraries[0][0].segments.length == 1 ? true : false
        item.service = item.services["0PC"] ? false : true
      })
      let filterOptions = this.rateOption.filter(item => item.state).map(i => i.code)
      let filterCompany = this.airlines.filter(item => item.state).map(i => i.code).length == 0
        ? this.airlines.map(i => i.code)
        : this.airlines.filter(item => item.state).map(i => i.code)
      return TICKETSDATA.flights.filter(item => {
        return filterOptions.map(i => item[i]).every(el => el) && filterCompany.find(company => item.itineraries[0][0].carrier == company)
      })
    }
  },
  methods: {
    clearRateOption(){
      this.rateOption.map(item => {item.state = false})
    },
    clearAirline(){
      this.airlines.map(item => {item.state = false})
    },
    filterMobileClick(){
      this.filterMobileState = !this.filterMobileState
    }
  },
  created() {
    this.airlines =  Object.keys(TICKETSDATA.airlines).map(keys => ({
      name: TICKETSDATA.airlines[keys],
      state: false,
      code: keys
    }))
  }
}
</script>

<style lang="scss" scoped>
@import '~/assets/style/_mixins/values.scss';
.main{
  padding-bottom: 15rem;
  .grid-container-gap{
    display: grid;
    grid-row-gap: 1.2rem;
    height: max-content;
  }
  .content{
    margin-top: 2rem;
    @media screen and (max-width: 1024px){
      .tickets-filter{
        display: none;
      }
      .tickets-box{
        grid-column: 1 / 13;
      }
    }
  }

  .tickets-filter-popup{
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    overflow-x: hidden;
    overflow-y: scroll;
    background-color: $bg-light-color;
    .tickets-filter-popup__close{
      position: fixed;
      bottom: 2rem;
      right: 0;
      left: 0;
      margin: auto;
      width: max-content;
      border-radius: 2.5rem;
      background-color: $dark-color;
      padding: 1rem;
    }
  }
  .tickets-filter_mobile{
    position: fixed;
    bottom: 2rem;
    right: 0;
    left: 0;
    margin: auto;
    width: max-content;
    border-radius: 2.5rem;
    display: flex;
    align-items: center;
    padding: 1.6rem 2rem;
    background-color: $dark-color;
    box-shadow: 0 2px 8px 0 rgba(0,0,0,.2);
    .new-search{
      padding-right: 1.2rem;
      margin-right: 1.2rem;
      border-right: .1rem solid white;
      p{
        color: white;
        font-weight: 600;
      }
    }
    .tickets-filter{
      display: flex;
      align-items: center;
      &:hover{
        cursor: pointer;
      }
      p{
        margin-left: .8rem;
        color: white;
        font-weight: 600;
      }
    }
    @media screen and (min-width: 1024px){
      display: none;
    }
  }
}


</style>
