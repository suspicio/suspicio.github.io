<template>
  <div class="special__project">
    <img class="left__image" src="../../assets/prizes_left_side.png">
    <img class="right__image" src="../../assets/prizes_right_side.png">
    <img class="left__image__small" src="../../assets/small_specproject_left.png">
    <img class="right__image__small" src="../../assets/small_specproject_right.png">
    <centralized-heading
      class="size"
      :text="'Конкурс'"
      :text-color="'white'"
    ></centralized-heading>
    <div class="content__box">
      <div>
        <img v-if="!small" src="../../assets/spec_photo.png">
        <img v-else src="../../assets/small_spec_photo.png">
      </div>
      <div class="text__content">
        <h1>Повторяй фотографии своих родителей в новогодней стилистике</h1>
        <p>Выкладывай у себя в Instagram,указывай #РетроЧелленджМагнит в описании к посту и получи шанс выиграть артефакты советских времен</p>
      </div>
    </div>
  </div>
</template>

<script>
import CentralizedHeading from '@/components/CentralizedHeading'

export default {
  name: 'SpecialProject',
  components: {
    CentralizedHeading
  },
  mounted () {
    window.addEventListener('resize', this.onResize, true)
    this.small = window.innerWidth <= 1000
  },
  data () {
    return {
      small: false
    }
  },
  methods: {
    onResize () {
      this.small = window.innerWidth <= 1000
    }
  },
  beforeDestroy () {
    window.removeEventListener('resize', this.onResize, true)
  }
}
</script>

<style lang="scss" scoped>
.special__project {
  height: fit-content;
  position: relative;
  overflow: hidden;
  background-color: #D02E26;
}

.text__content {
  display: flex;
  flex-direction: column;
  padding-left: 80px;
  color: white;
  padding-bottom: 100px;

  h1 {
    font-family: Poiret One;
    font-style: normal;
    font-weight: normal;
    font-size: 27px;
    line-height: 32px;
    margin-bottom: 27px;
  }

  p {
    font-family: 'Zen Kaku Gothic New', sans-serif;
    font-style: normal;
    font-weight: normal;
    font-size: 18px;
    line-height: 26px;
    margin-bottom: 36px;
  }
}

.left__image {
  position: absolute;
  left: 0;
  top: 0;

  &__small {
    display: none;
  }
}

.right__image {
  position: absolute;
  right: 0;
  top: 0;

  &__small {
    display: none;
  }
}

.content__box {
  width: 100%;
  max-width: 1000px;
  margin-top: 27px;
  display: flex;
  justify-content: center;
  align-items: center;
  margin-left: auto;
  margin-right: auto;
  z-index: 1;
}

@media (max-width: 1200px) {
  .text__content {
    padding-left: 0;
  }

  .left__image {
    display: none;
  }

  .right__image {
    display: none;
  }
}

@media (max-width: 1024px) {
  .size {
    margin-top: -25px;
  }

  .left__image__small {
    display: block;
    position: absolute;
    top: 0;
    left: 0;
    z-index: 0;
  }

  .right__image {
    display: none;

    &__small {
      display: block;
      position: absolute;
      top: 0;
      right: 0;
      z-index: 0;
    }
  }

  .content__box {
    display: flex;
    flex-direction: column-reverse;

    .the__button {
      margin: 0 30px 30px;
      width: 100%;
    }

    div {
      display: flex;
      justify-content: center;
      padding-bottom: 10px;
      z-index: 1;
    }
  }

  .text__content {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    text-align: left;
    padding: 0 10px 0;

    h1 {
      font-size: 17px;
      width: 100%;
      line-height: 20px;
    }

    p {
      font-size: 14px;
      line-height: 20px;
    }
  }
}
</style>
