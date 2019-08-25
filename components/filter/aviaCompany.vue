<template>
  <div class="rate-option">
    <div class="head">
      <h4 class="head-text">Авиакомпании</h4>
      <div v-if="clearVisible"  @click="resetChoice()">
        <reset-choice />
      </div>
    </div>

    <div class="selects">

      <label :for="all.code" class="selects-item" @click="resetChoice">
        <input :disabled="true" class="input-checkbox" v-model="checkBoxAllDisabled" type="checkbox" :id="all.code">
        <div class="check-box">
          <svg width="8" height="8" viewBox="0 0 8 8" fill="none" xmlns="http://www.w3.org/2000/svg">
            <path fill-rule="evenodd" clip-rule="evenodd" d="M7.95452 2.1527L3.24992 7.44537L0 4.19545L1.41421 2.78123L3.16429 4.53131L6.4597 0.823975L7.95452 2.1527Z" fill="#B9B9B9"/>
          </svg>

        </div>

        <p>{{all.name}}</p>
      </label>


      <label v-for="(item, index) in selects" :key="index" :for="item.code" class="selects-item">
        <input class="input-checkbox" v-model="item.state" type="checkbox" :id="item.code">
        <div class="check-box">
          <svg width="8" height="8" viewBox="0 0 8 8" fill="none" xmlns="http://www.w3.org/2000/svg">
            <path fill-rule="evenodd" clip-rule="evenodd" d="M7.95452 2.1527L3.24992 7.44537L0 4.19545L1.41421 2.78123L3.16429 4.53131L6.4597 0.823975L7.95452 2.1527Z" fill="#B9B9B9"/>
          </svg>

        </div>

        <p>{{item.name}}</p>
      </label>
    </div>
  </div>
</template>

<script>
  import ResetChoice from '~/components/icons/resetChoice'
  export default {
    name: "avia-company",
    components: {
      ResetChoice
    },
    data: () => ({
      all: {
        name: 'Все',
        state: true,
        code: 'all'
      }
    }),
    props: {
      selects: {
        type: Array,
        required: true
      }
    },
    computed: {
      checkBoxAllDisabled(){
        return !this.selects.find(item => item.state == true)
      },
      clearVisible(){
        return this.selects.find(i => i.state == true)
      }
    },
    methods: {
      resetChoice(){
        this.$emit('resetChoice')
      },

    }

  }
</script>

<style lang="scss" scoped>
  @import "~/assets/style/_mixins/values.scss";
  .rate-option{
    background-color: $bg-light-color;
    border-radius: $border-radius;


    .head{
      padding: 1.2rem;
      display: flex;
      align-items: center;
      justify-content: space-between;
      &-text{
        font-weight: bold;
      }
    }
    .selects{
      padding-bottom: 1.2rem;
      user-select: none;
      &-item{
        padding: .8rem 1.2rem;
        display: flex;
        align-items: center;
        .input-checkbox{
          display: none;
          &:checked + .check-box{
            background-color: $green-color;
            svg{
              display: block;
              path{
                fill: white
              }
            }
          }
        }

        .check-box{
          margin-right: 1.2rem;
          border: 1px solid #B9B9B9;
          box-sizing: border-box;
          border-radius: 2px;
          width: 1.2rem;
          height: 1.2rem;
          display: flex;
          align-items: center;
          justify-content: center;
          svg{
            display: none;
          }
        }
        &:hover{
          background-color: #EBEBEB;
          .check-box{
            svg{
              display: block;
            }
          }
        }

      }
    }
  }


</style>
