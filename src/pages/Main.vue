<template>
  <div class="main">
    <TheHeader @activate="registrationVisible"/>
    <Registration v-if="isRegistrationActive" @activate="registrationVisible"
                  @activateLogIn="activateLogIn"/>
    <LogIn v-if="isActiveLogIn" :isNumber="isNumberShown" :number="numberPass" @activateForgot="activateForgot"
           @activateLogIn="activateLogIn"/>
    <ForgotPass v-if="isActiveForgot" @activateForgot="activateForgot" @sendToNumber="sendPass"/>
    <CheckRegistrationOptions v-if="isActiveCheckRegOpt" @activate="activateCheckRegOpt"
                              @manual="activateManualCheck" @qrcode="activateQRCode"/>
    <ManualCheckEnter v-if="isActiveManualCheck" @activate="activateManualCheck" @checkStatus="checkStatus"/>
    <CheckLoader v-if="isLoading" @activate="activateLoading" @checkStatus="checkStatus"/>
    <QRCodeReader v-if="isActiveQR" @activate="activateQRCode"/>
    <EmailConfirmation v-if="isMail" @activate="activateMail"/>
    <CheckStatus
      v-if="!isLoading && isSended"
      :activate="activateStatus"
      :button-text="'ЛИЧНЫЙ КАБИНЕТ'"
      :header-text="'Чек зарегистрирован'"
      :is-ok="true"
      :under-text="'Пожалуйста, сохраните чек до конца акции'"
    />
    <CheckRegistration
      @activateOptions="activateCheckRegOpt"
    />
    <ParticipateInShare id="actions"/>
    <WinThePrize id="prizes"/>
    <SpecialProject id="gallery"/>
    <TheParticipants :photo="photo"/>
    <ProductionInAction id="products"/>
    <TheWinners :winners="winners"/>
    <FAQ id="faq"/>
    <TheFooter/>
  </div>
</template>

<script>
import TheHeader from '@/components/TheHeader'
import CheckRegistration from '@/views/Main/CheckRegistration'
import ParticipateInShare from '@/views/Main/ParticipateInShare'
import WinThePrize from '@/views/Main/WinThePrize'
import SpecialProject from '@/views/Main/SpecialProject'
import TheParticipants from '@/views/Main/TheParticipants'
import ProductionInAction from '@/views/Main/ProductionInAction'
import TheFooter from '@/components/TheFooter'
import TheWinners from '@/views/Main/TheWinners'
import Registration from '@/views/Auth/Registration'
import LogIn from '@/views/Auth/LogIn'
import ForgotPass from '@/views/Auth/ForgotPass'
import CheckRegistrationOptions from '@/views/CheckRegister/CheckRegistrationOptions'
import ManualCheckEnter from '@/views/CheckRegister/ManualCheckEnter'
import CheckLoader from '@/views/CheckRegister/CheckLoader'
import CheckStatus from '@/views/CheckRegister/CheckStatus'
import { mapActions, mapState } from 'vuex'
import FAQ from '@/views/Main/FAQ'
import QRCodeReader from '@/views/CheckRegister/QRCodeReader'
import EmailConfirmation from '@/views/Auth/EmailConfirmation'
import axios from 'axios'
import Vue from 'vue'

export default {
  name: 'Main',
  components: {
    EmailConfirmation,
    QRCodeReader,
    FAQ,
    CheckStatus,
    CheckLoader,
    ManualCheckEnter,
    CheckRegistrationOptions,
    ForgotPass,
    LogIn,
    Registration,
    TheParticipants,
    TheFooter,
    ProductionInAction,
    TheWinners,
    SpecialProject,
    WinThePrize,
    ParticipateInShare,
    CheckRegistration,
    TheHeader
  },
  data () {
    return {
      isRegistrationActive: false,
      isActiveLogIn: false,
      isActiveForgot: false,
      isActiveCheckRegOpt: false,
      isActiveManualCheck: false,
      isLoading: false,
      isSended: false,
      isActiveQR: false,
      isNumberShown: false,
      isMail: false,
      numberPass: ''
    }
  },

  computed: {
    ...mapState({
      user: state => state.user,
      winners: state => state.winners,
      photo: state => state.photo
    })
  },

  created () {
    if (this.$root.$route.query['email-confirmation'] !== undefined) {
      axios.post('/email-confirmation', this.$root.$route.query['email-confirmation'])
        .then(res => {
          if (res.data.success) {
            this.isMail = true
          } else {
            Vue.prototype.$toasted.error(res.data.error)
          }
        })
        .catch(e => {
          console.error(e)
        })
    }

    this.getPhoto()
  },

  methods: {
    ...mapActions(['getPhoto']),

    registrationVisible () {
      this.isNumberShown = false
      this.isRegistrationActive = !this.isRegistrationActive
    },

    activateLogIn () {
      this.isNumberShown = false
      this.isActiveLogIn = !this.isActiveLogIn
    },

    activateForgot () {
      this.isNumberShown = false
      this.isActiveForgot = !this.isActiveForgot
    },

    activateCheckRegOpt () {
      this.isNumberShown = false
      if (this.user) {
        this.isActiveCheckRegOpt = !this.isActiveCheckRegOpt
      } else {
        this.isRegistrationActive = true
      }
    },

    activateManualCheck () {
      this.isNumberShown = false
      this.isActiveManualCheck = !this.isActiveManualCheck
    },

    activateQRCode () {
      this.isActiveQR = !this.isActiveQR
    },

    checkStatus (promise) {
      this.isNumberShown = false
      this.isActiveManualCheck = false
      this.isLoading = true
      promise.then(data => {
        this.isLoading = false
      })
    },

    activateLoading () {
      this.isNumberShown = false
      this.isLoading = !this.isLoading
    },

    sendPass (number) {
      this.isActiveForgot = !this.isActiveForgot
      this.isActiveLogIn = !this.isActiveLogIn
      this.isNumberShown = true
      this.numberPass = number
    },

    activateStatus () {
      this.isSended = !this.isSended
    },

    activateMail () {
      this.isMail = !this.isMail
    }
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Poiret+One&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Zen+Kaku+Gothic+New:wght@300;400;500;700;900&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Montserrat:ital,wght@0,100;0,200;0,300;0,400;0,500;0,600;0,700;0,800;0,900;1,100;1,200;1,300;1,400;1,500;1,600;1,700;1,800;1,900&display=swap');

* {
  margin: 0;
  padding: 0;
  border: 0;
  box-sizing: border-box;
}

.main {
  display: flex;
  flex-direction: column;
}
</style>
