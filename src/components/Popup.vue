<template>
  <div class="popup" v-show="isShow">
    <div class="popup__close" @click="closeModal">&times;</div>
    <div class="popup__title">{{ info.title }}</div>
    <div class="popup__description">{{ info.description }}</div>
    <div class="popup__titleInput">
      {{ info.titleInput }}
    </div>
    <input
      type="text"
      class="popup__text"
      placeholder="Введите данные"
      @input="setPay($event.target.value)"
    />
    <div class="popup__calc" @click="getPartOfPay()" v-show="Number(salary) > minValue">
      {{ info.calc }}
    </div>
    <div class="popup__check_title" v-show="isShowLong">
      {{ info.checkTitle }}
    </div>
    <Checkbox
      v-for="(pay, index) of payment"
      :key="index"
      :part="pay.pt"
      :numberYear="pay.numberYear"
      v-show="isShowLong"
    />
    <div class="popup__info">
      <div class="popup__info_question">{{ info.question }}</div>
      <div class="popup__info_wrapper">
        <button class="popup__info_pay popup__info_pay_active">{{ info.pay }}</button>
        <button class="popup__info_time">{{ info.time }}</button>
      </div>
    </div>
    <button class="popup__add" :disabled="isDisabled" :class="{ disable: isDisabled }">
      {{ info.add }}
    </button>
  </div>
</template>

<script>
import Checkbox from './Checkbox.vue';

export default {
  name: 'Popup',
  components: {
    Checkbox
  },
  data() {
    return {
      payment: [
        { pt: 0, numberYear: 'в 1-ый год' },
        { pt: 0, numberYear: 'во 2-ой год' },
        { pt: 0, numberYear: 'в 3-ий год' },
        { pt: 0, numberYear: 'в 4-ый год' }
      ],
      info: {
        title: 'Налоговый вычет',
        description:
          'Используйте налоговый вычет чтобы погасить ипотеку досрочно. Размер налогового вычета составляет не более 13% от своего официального годового дохода.',
        titleInput: 'Ваша зарплата в месяц',
        calc: 'Рассчитать',
        checkTitle: 'Итого можете внести в качестве досрочных:',
        question: 'Что уменьшаем ?',
        pay: 'Платеж',
        time: 'Срок',
        add: 'Добавить'
      },
      salary: 0,
      maxValue: 260000,
      minValue: 33000,
      isShowLong: false,
      isClosed: true,
      isDisabled: 'disabled'
    };
  },
  methods: {
    setPay(value) {
      this.salary = value;
      value < this.minValue ? (this.isDisabled = 'disabled') : (this.isDisabled = null);
    },
    getPartOfPay() {
      this.payment.map((pay) => (pay.pt = this.salary * 12 * 0.13));
      switch (Math.floor(this.maxValue / (this.salary * 12 * 0.13))) {
        case 3:
          this.payment.pop();
          this.payment.push({
            pt:
              this.maxValue -
              Math.floor(this.maxValue / (this.salary * 12 * 0.13)) * (this.salary * 12 * 0.13),
            numberYear: 'в 4-ый год'
          });
          break;
        case 2:
          this.payment.splice(2, 3);
          this.payment.push({
            pt:
              this.maxValue -
              Math.floor(this.maxValue / (this.salary * 12 * 0.13)) * (this.salary * 12 * 0.13),
            numberYear: 'в 3-ий год'
          });
          break;
        case 1:
          this.payment.splice(1, 3);
          this.payment.push({
            pt:
              this.maxValue -
              Math.floor(this.maxValue / (this.salary * 12 * 0.13)) * (this.salary * 12 * 0.13),
            numberYear: 'во 2-ой год'
          });
          break;
      }
      this.isShowLong = true;
    },
    closeModal() {
      this.$emit('close', this.isClosed);
      this.isShowLong = false;
    }
  },
  props: ['isShow']
};
</script>

<style lang="scss">
.popup {
  position: absolute;
  padding: 32px;
  width: 552px;
  min-height: 476px;
  background: #ffffff;
  border-radius: 30px;
  z-index: 5;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  transistion: all 0.5s;
  &__title {
    font-weight: 500;
    color: #000000;
    font-size: 28px;
    line-height: 40px;
    margin-bottom: 16px;
  }
  &__check {
    &_title {
      font-weight: 500;
      font-size: 14px;
      line-height: 24px;
      color: #000000;
      margin-bottom: 16px;
    }
  }
  &__description {
    font-weight: 400;
    font-size: 14px;
    line-height: 24px;
    color: #808080;
    margin-bottom: 24px;
  }
  &__titleInput {
    font-weight: 500;
    font-size: 14px;
    line-height: 24px;
    color: #000000;
    margin-bottom: 8px;
  }
  &__text {
    width: 100%;
    height: 40px;
    padding: 8px 0 8px 10px;
    border-radius: 5px;
    margin-bottom: 8px;
    border: 1px solid #808080;
    &:hover {
      border: 1px solid #000000;
    }
  }
  &__calc {
    font-weight: 500;
    font-size: 14px;
    line-height: 24px;
    color: #ea0029;
    margin-bottom: 16px;
    cursor: pointer;
    &:hover {
      color: #f53a31;
    }
  }
  &__info {
    display: flex;
    justify-content: flex-start;
    align-items: center;
    margin-bottom: 40px;
    &_question {
      font-weight: 500;
      font-size: 14px;
      line-height: 24px;
      color: #000000;
      margin-right: 24px;
    }
    &_pay,
    &_time {
      border-radius: 50px;
      padding: 6px 12px;
      height: 36px;
      background: #eef0f2;
      color: #000000;
      border: none;
      cursor: pointer;
      margin: 0 8px;
    }
    &_pay {
      width: 73px;
      &_active {
        color: #ffffff;
        background: linear-gradient(255.35deg, #dc3131 0.83%, rgba(255, 79, 79, 0) 108.93%), #ff5e56;
      }
    }
    &_time {
      width: 57px;
    }
  }
  &__add {
    display: inline;
    width: 488px;
    height: 56px;
    color: #ffffff;
    background: linear-gradient(255.35deg, #dc3131 0.83%, rgba(255, 79, 79, 0) 108.93%), #ff5e56;
    box-shadow: 0px 0px 24px rgba(234, 0, 41, 0.33);
    border-radius: 6px;
    border: none;
    cursor: pointer;
    font-weight: 500;
    font-size: 16px;
    line-height: 24px;
    &:hover {
      background: #ea0029;
    }
    &:active {
      background: #ea0029;
    }
  }
  &__close {
    position: absolute;
    color: #ea0029;
    font-size: 40px;
    font-weight: 500;
    top: 5px;
    right: 27px;
    cursor: pointer;
  }
}

.disable {
  background: #bec5cc;
  box-shadow: none;
  &:hover {
    background: #bec5cc;
  }
}

@media (max-width: 768px) {
  .popup {
    width: 453px;
    &__add {
      width: 389px;
      margin: 0 auto;
    }
    &__info {
      margin-bottom: 48px;
    }
  }
}

@media (max-width: 455px) {
  .popup {
    width: 100vw;
    height: 100vh;
    border-radius: 0;
    &__add {
      width: 100%;
    }
  }
}

@media (max-width: 361px) {
  .popup {
    &__close {
      right: 10px;
    }
    &__info {
      flex-direction: column;
      align-items: flex-start;
      &_wrapper {
        margin-top: 24px;
        display: flex;
      }
      &_pay,
      &_time {
        margin: 0;
      }
      &_pay {
        margin-right: 8px;
      }
    }
  }
}
</style>
