<template>
  <div class="wrapper-card">
    <div class="card" @click="toInput">
      <div class="card__wrapper">
        <div class="card__watermark"></div>
        <h3 class="card__number">{{ formatCardNumber(cardFields.card) }}</h3>
        <div class="card__menu menu">
          <div class="menu__holder">
            <h3 class="menu__title">Card Holder</h3>
            <h3 class="menu__info">{{ userName }}</h3>
          </div>

          <div class="menu__expires">
            <h3 class="menu__title">Expires</h3>
            <h3 class="menu__info">{{ cardMonth }}/{{ cardYear }}</h3>
          </div>
        </div>
      </div>
    </div>

    <div class="payment">
      <div class="payment__name">
        <h3 class="payment__title">Name on card</h3>
        <input ref="userName" type="text" class="payment__input" v-model="cardFields.userName">
      </div>

      <div class="payment__number">
        <h3 class="payment__title">Card number</h3>
        <input type="text" class="payment__input" v-model="cardFields.card" maxlength="19">
      </div>

      <div class="payment__card-date">
        <div class="payment__date">
          <h3 class="payment__title">Expiration date</h3>
          <div class="payment__dates">
            <input  type="text" class="payment__input" placeholder="Month(1-12)" v-model="cardFields.cardMonth" maxlength="2">
            <input type="text" class="payment__input" placeholder="Year(23-29)" v-model="cardFields.cardYear" maxlength="2">
          </div>
        </div>

        <div class="payment__cvc">
          <h3 class="payment__title">CVC</h3>
          <input type="text" class="payment__input" maxlength="3" v-model="cardFields.CVC">
        </div>
      </div>

      <h3 class="payment__total">000 USD</h3>

      <button :disabled="!disabledBtn" class="payment__btn" @click="succsess">Confirm payment</button>
    </div>
  </div>
</template>


<script>
export default {
  data() {
    return {
      cardFields: {
        userName: '',
        card: '',
        cardMonth: '',
        cardYear: '',
        CVC: ''
      }
    }
  },
  computed: {
    userName() { 
      return this.cardFields.userName || 'FULL NAME' 
    },
    cardNumber() { 
      return this.cardFields.card || '#### #### #### ####' 
    },
    cardMonth() {
      if (typeof +this.cardFields.cardMonth === 'number' && this.cardFields.cardMonth > 0 && this.cardFields.cardMonth < 13) {
        return this.cardFields.cardMonth
      } else {
        return 'MM'
      }
    },
    cardYear() {
      if (typeof +this.cardFields.cardYear === 'number' && this.cardFields.cardYear > 22 && this.cardFields.cardYear < 30) {
        return this.cardFields.cardYear 
      } else {
        return 'YY'
      }
    },
    disabledBtn() { 
      return Object.values(this.cardFields).every(e => e) 
    }
  },
  methods: {
    formatCardNumber(value) { 
      const regex = /^(\d{0,4})(\d{0,4})(\d{0,4})(\d{0,4})$/g 
      const onlyNumbers = value.replace(/[^\d]/g, '') 
      return onlyNumbers.replace(regex, (regex, $1, $2, $3, $4) => [$1, $2, $3, $4].map(group => group.length < 4 ? group + '#'.repeat(4 - group.length) : group).join(' ')) 
    },
    toInput() { 
      this.$refs.userName.focus() 
    },
    succsess() { 
      Object.keys(this.cardFields).forEach(e => this.cardFields[e] = '')
    }
  }
}
</script>

<style scoped lang="scss">
h3 {
  color: #fff;
  font-weight: 400;
  margin: 0;
  letter-spacing: 0.05rem;
}
input {
  max-width: 45rem;
}
button:hover {
  cursor: pointer;
}
button:disabled {
  cursor: not-allowed;
  background: transparent;
  border: 2px solid #64758A;
  color: #64758A;
}
.wrapper-card {
  max-width: 50rem;
  margin: 0 auto;
  padding: 3rem 4.5rem;
  background: #fff;
  border-radius: 7px;
  box-shadow: 0 0 10px rgb(103, 103, 103);
}
.card:hover {
  cursor: pointer;
}
.payment h3 {
  color: #64758A;
}
h3.payment__total {
  color: #000;
  font-weight: 500;
}
.payment {
  max-width: 47rem;
  &__title {
    font-size: 0.9rem;
    letter-spacing: 0.01rem;
    margin-bottom: 0.5rem;
  }
  &__input {
    width: 100%;
    max-width: 43.6rem;
    height: 2rem;
    outline: none;
    border: 1px solid #64758A;
    border-radius: 5px;
    margin-bottom: 1rem;
    padding: 0.3rem 1.2rem;
  }
  &__input::placeholder {
    letter-spacing: 0.1rem;
  }
  &__card-date {
    display: flex;
    align-items: center;
    margin-bottom: 1rem;
  }
  &__card-date input {
    max-width: 12.5rem;
    margin-right: 3rem;
  }
  &__dates {
    display: flex;
    align-items: center;
  }
  &__total {
    display: flex;
    justify-content: end;
    margin-bottom: 1rem;
  }
  &__btn {
    background: #2790D8;
    outline: none;
    border: none;
    padding: 1rem 1.5rem;
    width: 100%;
    color: #fff;
    border-radius: 5px;
    letter-spacing: 0.05rem;
  }
  &__btn:active {
    box-shadow: inset 1px 1px 3px rgb(49, 49, 49);
  }
}
.menu {
  display: flex;
  align-items: center;
  justify-content: space-between;
  &__title {
    font-size: 0.7rem;
    opacity: 0.85;
    margin-bottom: 0.2rem;
  }
  &__info {
    font-size: 0.9rem;
    max-width: 15rem;
    overflow: hidden;
  }
}
.card {
  width: 24.375rem;
  height: 13.125rem;
  background: url('@/assets/images/background.jpg');
  background-repeat: no-repeat;
  background-size: cover;
  margin: 0 auto;
  border-radius: 10px;
  box-shadow: 0 0 20px #000;
  margin-bottom: 3rem;
  &__wrapper {
    padding: 20px;
  }
  &__watermark {
    width: 3.4rem;
    height: 2.15rem;
    outline: 1px solid #64758A;
    outline-offset: -3px;
    background: url('@/assets/images/watermark.png');
    background-repeat: no-repeat;
    border-radius: 5px;
    margin-bottom: 2.5rem;
  }
  &__number {
    word-spacing: 1.2rem;
    font-size: 1.4rem;
    margin-bottom: 2rem;
  }
}

@media (max-width: 33.125em) {
  .wrapper-card {
    width: 100%;
    padding: 0.8rem 0;
    box-shadow: 0 0 5px rgb(103, 103, 103);
    border-radius: 10px;
  }
  .menu {
    &__title {
      font-size: 0.6rem;
      margin-bottom: 0.2rem;
    }
    &__info {
      font-size: 0.9rem;
      max-width: 9rem;
      overflow: hidden;
    }
  }
  .card {
    width: 18.375rem;
    height: 10.125rem;
    margin-bottom: 2rem;
    &__wrapper {
      padding: 10px;
    }
    &__watermark {
      width: 2.4rem;
      height: 1.55rem;
      margin-bottom: 2rem;
    }
    &__number {
      word-spacing: 0.2rem;
      font-size: 1.3rem;
      margin-bottom: 1.5rem;
    }
  }
  .payment {
    max-width: 20rem;
    text-align: center;
    margin: 0 auto;
    &__title {
      font-size: 1.1rem;
    }
    &__input {
      width: 14rem;
      margin: 0;
      margin-bottom: 1rem;
    }
    &__card-date {
      display: flex;
      flex-direction: column;
    }
    &__card-date input {
      max-width: 15rem;
      margin: 0;
      margin-bottom: 1rem;
    }
    &__dates {
      display: flex;
      flex-direction: column;
      align-items: center;
    }
    &__total {
      display: flex;
      justify-content: center;
      margin-top: -1rem;
      margin-bottom: 1rem;
    }
    &__btn {
      width: 16rem;
    }
  }
}
@media (min-width: 90.625em){
  .wrapper-card {
    margin: 0 auto;
  }
}
</style>