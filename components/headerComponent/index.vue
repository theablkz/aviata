<template>
  <div class="header">
    <div class="header-box">
      <a href="https://aviata.kz/">
        <img class="image" :src="logo" alt="">
      </a>
      <div class="reserv-tickets">
        <h3 class="reserv-tickets__title">Поиск и бронирование авиабилетов</h3>
        <div class="reserv-tickets-steps">
          <div v-for="(item, index) in getSteps" :key="index"
               class="reserv-tickets-steps__box"
          >
            <p class="reserv-tickets-steps__items"
               :class="{active: item.active}"
            >{{item.name}}</p>
            <arrow v-if="getSteps.length-1 > index" class="arrow"/>
          </div>
        </div>
      </div>

      <div class="header-contacts">
        <a href="tel: +77272255055"><h4 class="arial">+7(727)<span>22-55-055</span></h4></a>
        <a href="tel: +77272255055"><h4 class="arial">+7(701)065-33-33</h4></a>
        <a href="tel: +77272255055"><h4 class="arial">help@aviata.kz</h4></a>
      </div>
    </div>

  </div>
</template>

<script>
  import Arrow from '~/components/icons/arrow'
  export default {
    components: {
      Arrow
    },
      data: () => ({
        logo: require("~/assets/img/logo.svg"),
        steps: [
          {name: 'выбор варианта', address: '/'},
          {name: 'пассажиры', address: '/2'},
          {name: 'способ оплаты', address: '/3'},
          {name: 'подтверждение', address: '/4'},
        ]
      }),
    computed: {
        getSteps(){
          this.steps.find(item => item.address == this.$route.path).active = true
          return this.steps
        }
    }
  }
</script>

<style lang="scss" scoped>
  @import "~/assets/style/_mixins/values.scss";
.header{
  background-color: white;
  .header-box{
    max-width: 90rem;
    margin: auto;
    display: grid;
    grid-template-areas: "logo reserv contacts";
    grid-column-gap: 1rem;
    align-items: center;
    padding: 2rem 1.6rem;
    @media screen and (max-width: 900px){
      grid-template-areas: "logo  contacts"
                            "reserv reserv";
    }
    .reserv-tickets{
      grid-area: reserv;
      justify-self: center;
      @media screen and (max-width: 500px) {
        display: none;
      }
      .reserv-tickets__title{
        margin-bottom: 1rem;
      }
      &-steps{
        display: flex;
        align-items: center;
        flex-wrap: wrap;
        .reserv-tickets-steps__box{
          display: flex;
          align-items: center;
          .reserv-tickets-steps__items{
            border-radius: $border-radius;
            padding: .4rem;
            background-color: $deep-gray-color;
            font-weight: 600;
            &.active{
              background-color: $green-color;
              color: white;
            }
          }
          .arrow{
            margin: 0 .5rem;
            width: .9rem;
            height: .9rem;
          }
        }

      }
    }
    .header-contacts{
      grid-area: contacts;
      justify-self: end;
      @media screen and (max-width: 500px) {
        display: none;
      }
      h4{
        text-align: right;
        line-height: 2.2rem;
        span{
          font-size: 2.4rem;
        }
      }
    }
  }

}

  .image{
    max-width: 24rem;
    height: auto;
    grid-area: logo;
    @media screen and (max-width: 500px) {
      max-width: 18rem;
    }
  }
</style>
