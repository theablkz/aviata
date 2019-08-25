<template>
    <div class="tickets">

      <div class="tickets-info">
        <div class="tickets-info__head">
          <div class="head-company-name">
            <img class="head-company-name__logo" :src="carrierLogo" alt="">
            <h4 class="head-company-name__name">{{carrier.carrier_name}}</h4>
          </div>
          <p class="fly-detail">Детали перелета</p>
        </div>
        <div class="tickets-info__time">
          <div class="date-fly date-fly_departure">
            <p>{{departureDate}}</p>
            <h3 class="date-fly__time">{{departureTime}}</h3>
          </div>
          <div class="flight-info">
            <div class="flight-info-visual">
              <div class="flight-info-visual__info">
                <p class="city-abriviature">{{carrier.segments[0].origin_code}}</p>
                <p>{{travelTime}}</p>
                <p class="city-abriviature">{{carrier.segments[carrier.segments.length - 1].dest_code}}</p>
              </div>
              <svg class="image" width="173" height="5" viewBox="0 0 173 5" fill="none" xmlns="http://www.w3.org/2000/svg">
                <g clip-path="url(#clip0)">
                  <path d="M2.43799 2.5L170.438 2.5" stroke="#B9B9B9"/>
                  <circle cx="169.938" cy="2.5" r="2" fill="white" stroke="#B9B9B9"/>
                  <circle cx="2.93799" cy="2.5" r="2" fill="white" stroke="#B9B9B9"/>
                </g>
                <circle cx="83.938" cy="2.5" r="2" fill="white" stroke="#B9B9B9"/>
                <defs>
                  <clipPath id="clip0">
                    <rect width="172" height="5" fill="white" transform="translate(0.437988)"/>
                  </clipPath>
                </defs>
              </svg>

            </div>
            <div class="flight-info-text">
                <p class="flight-info-text__name" :class="{'direct-flight': item.directFlight}">{{directFlightText}}</p>
            </div>
          </div>
          <div class="date-fly date-fly_arrival">
            <p>{{arriveDate}}</p>
            <h3 class="date-fly__time">{{arriveTime}}</h3>
          </div>
          <div class="fly-detail--mobile-none">
            <p class="fly-detail">Условия тарифа</p>
          </div>
          <div v-if="!item.refundable" class="refundable-detail fly-detail--mobile-none">
            <img class="image refundable-detail__icon" :src="nonRefunDebler" alt="">
            <p class="fly-detail">невозвратный</p>
          </div>
        </div>
      </div>
      <div class="tickets-price">
        <h3 class="arial tickets-price__info">{{item.price}} ₸</h3>
        <button class="ticket-buy-button">Выбрать</button>
        <p class="tickets-passager-price">Цена за всех пассажиров</p>
        <div class="tickets-service">
          <div v-if="item.service" class="service" :style="{backgroundImage:  `url(${baggageIcon})` }">
            <p v-for="item in Object.keys(item.services)">{{item}}</p>
          </div>
          <p v-else>Нет багажа</p>
          <button class="tickets-service__add">+ Добавить багаж</button>
        </div>
      </div>

    </div>
</template>

<script>
    export default {
        name: "tickets",
      data: () => ({
        dates: {
          mounth: [ "Янв", "Фев","Мар","Апр","Май", "Июн", "Июл", "Авг","Сен","Окт","Ноя",'Дек'],
          week: ['Вс','Пн','Вт','Ср','Чт','Пт','Сб']
        },
        nonRefunDebler: require("~/assets/img/non-refundebler.svg"),
        baggageIcon: require('~/assets/img/baggage-icon.svg')
      }),
      props: {
          item: {
            type: Object,
            required: true
          }
      },
      computed: {
          carrier(){
            return this.item.itineraries[0][0]
          },
          carrierLogo(){
            return `https://aviata.kz/static/airline-logos/80x80/${this.carrier.carrier}.png`
          },
          departureDate(){
            let date = new Date(this.carrier["dep_date"])
            return `${date.getDate()} ${this.dates.mounth[date.getMonth()]}, ${this.dates.week[date.getDay()]}`
          },
          departureTime(){
            let date = new Date(this.carrier["dep_date"])
            return `${(date.getHours() < 10 ? '0' : '') + date.getHours()}:${(date.getMinutes() < 10 ? '0' : '') + date.getMinutes()}`
          },
          arriveDate(){
            let date = new Date(this.carrier["arr_date"])
            return `${date.getDate()} ${this.dates.mounth[date.getMonth()]}, ${this.dates.week[date.getDay()]}`
          },
          arriveTime(){
            let date = new Date(this.carrier["arr_date"])
            return `${(date.getHours() < 10 ? '0' : '') + date.getHours()}:${(date.getMinutes() < 10 ? '0' : '') + date.getMinutes()}`
          },
          travelTime(){
              return `${(this.carrier.traveltime/60/60).toFixed()} ч ${(this.carrier.traveltime/60%60).toFixed()} м`
          },
        directFlightText(){
            let direct = {
              "true": "прямой рейс",
              "false": this.carrier.segments.length == 2 ? `через ${this.carrier.segments[1]["airport_dest"]}` : `${this.carrier.segments.length} пересадки`
            }
            return direct[this.item.directFlight]
        }

      }
    }
</script>

<style lang="scss" scoped>
@import '~/assets/style/_mixins/values.scss';

  .tickets{
    box-shadow: 0px 2px 4px rgba(0, 0, 0, 0.15);
    background-color: white;
    border-radius: $border-radius;
    display: grid;
    grid-template-columns: auto 24rem;
    grid-column-gap: 2rem;
    @media screen and (max-width: 768px){
      grid-template-columns: auto;
    }

    .tickets-info{
      display: grid;
      grid-template-columns: 14.4rem 1fr;
      @media screen and (max-width: 768px){
        grid-template-columns: auto;
      }
      .tickets-info__head{
        display: grid;
        grid-template-rows: 1fr auto;
        padding: 2rem;
        @media screen and (max-width: 768px){
          display: flex;
          align-items: center;
          justify-content: space-between;
        }
        .head-company-name{
          display: flex;
          align-items: center;
          .head-company-name__logo{
            width: 2rem;
            height: 2rem;
            object-fit: contain;
            margin-right: 1.2rem;
          }
          .head-company-name__name{
            font-weight: 600;
          }
        }

      }
      .tickets-info__time{
        display: grid;
        grid-template-columns: 1fr 1fr 1fr;
        grid-template-rows: 1fr auto;
        grid-column-gap: .8rem;
        justify-items: center;
        align-items: center;
        padding:  2rem 0;
        .date-fly{
          .date-fly__time{
            font-size: 1.6rem;
          }
        }
        .flight-info{
          .flight-info-visual{
            .flight-info-visual__info{
              display: flex;
              justify-content: space-between;
              align-items: center;

              .city-abriviature{
                font-size: 1rem;
                color: $gray-color;
              }
            }
          }
          .flight-info-text{
            .flight-info-text__name{
              color: $yellow-color;
              text-align: center;

              &.direct-flight{
                color: $green-color
              }
            }
          }
        }
      }
    }
    .tickets-price{
      padding: 1.2rem 2rem;
      display: flex;
      flex-direction: column;
      justify-content: space-between;
      align-items: center;
      background-color: #F5F5F5;
      .tickets-price__info{
        font-weight: normal;
        padding-bottom: 1.2rem;
      }
      .ticket-buy-button{
        margin-bottom: .8rem;
        font-size: 1.8rem;
        font-weight: bold;
        color: white;
        font-family: $primary-font ;
        border-radius: $border-radius;
        border: none;
        background-color: $green-color;
        width: 100%;
        padding: .8rem;
        &:hover{
          cursor: pointer;
        }
      }
      .tickets-passager-price{
        color: #707276;
        margin-bottom: 1.2rem;
      }
      .tickets-service{
        width: 100%;
        display: flex;
        align-items: center;
        justify-content: space-between;
        .tickets-service__add{
          background-color: $blue-light-color;
          color: $blue-color;
          font-family: $primary-font;
          font-weight: 600;
          border-radius: $border-radius;
          border: none;
          padding: .3rem .8rem;
          &:hover{
            cursor: pointer;
            background-color: #e3e9f3;
          }
        }

        .service{
          display: flex;
          align-items: center;
          justify-content: center;
          height: 2.5rem;
          width: 3rem;
          background-repeat: no-repeat;
          p{
            font-size: 1.1rem;
            color: white;
            font-weight: bold;
          }
        }
      }
    }

  }
  .fly-detail{
    color: $button-blue-color;
    border-bottom: .1rem dashed $button-blue-color;
    align-self: end ;
    width: max-content;
    &:hover{
      cursor: pointer;
    }

  }


  .refundable-detail{
    display: flex;
    align-items: center;
    .refundable-detail__icon{
      margin-right: .8rem;
    }
    .fly-detail{
      color: $dark-color;
      font-family: $secondary-font;
      border: none;
      align-self: center;
    }
  }
.fly-detail--mobile-none{
  @media screen and (max-width: 768px) {
    display: none;
  }
}
</style>
