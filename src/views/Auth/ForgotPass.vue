<template>
  <div class="modal__wrapper forgot-pass">
    <div class="modal__content">
      <div class="close" @click="disable">
      </div>
      <h1>ЗАБЫЛИ КОД?</h1>
      <form @submit.prevent="sendPass">
        <TheInput
          v-model="phone"
          :is-phone="true"
          :text="'Телефон (при регистрации)'"
          @input="(e) => {this.phone = e}"
        ></TheInput>
        <div class="form__buttons">
          <TheButton
            :bg-color="'#F8E577'"
            :is-rounded="true"
            :text="'ОТПРАВИТЬ'"
            class="TheButton"
          ></TheButton>
        </div>
      </form>
      <div v-if="isError" class="additional__info">{{ text }}</div>
    </div>
  </div>
</template>

<script>
import TheInput from '@/components/TheInput'
import TheButton from '@/components/TheButton'
import { mapActions } from 'vuex'

export default {
  name: 'ForgotPass',

  components: {
    TheButton,
    TheInput
  },

  data () {
    return {
      phone: '',
      text: '',
      isError: false
    }
  },

  methods: {
    ...mapActions(['askForSMSCode']),

    disable () {
      this.isError = false
      this.$emit('activateForgot')
    },

    sendPass () {
      this.askForSMSCode(this.phone?.replace('+7', '').replaceAll(' ', '')
        .replaceAll('-', '').replace('(', '')
        .replace(')', '').substr(0, 10) || '').then(value => {
        if (value.success === false) {
          this.text = value.error
          this.isError = true
        } else {
          this.$emit('sendToNumber', this.phone)
        }
      })
    }
  }
}
</script>

<style lang="scss" scoped>
.left__buttons {
  justify-content: left !important;
  align-items: start !important;
}

.forgot-pass {
  .TheButton {
    margin: 0 auto !important;
  }
}

.additional__info {
  position: absolute;
  width: fit-content;
  left: 50%;
  bottom: calc(100% + 10px);
  transform: translateX(-50%);
  font-family: Zen Kaku Gothic New, sans-serif;
  font-style: normal;
  font-weight: 400;
  font-size: 30px;
  line-height: 130%;
  text-align: center;
  text-transform: uppercase;
  color: #F8E577;
}

@media (max-width: 768px) {
  .additional__info {
    font-size: 22px !important;
    line-height: 110% !important;
    width: 220px;
    letter-spacing: 0.02em;
  }
}
</style>
