<template>
  <div class="contact-page">
    <img src="../assets/img/contact-bg-left.svg" alt="" class="contact-page__bg contact-page__bg--left">
    <img src="../assets/img/contact-bg-right.svg" alt="" class="contact-page__bg contact-page__bg--right">
    <div class="contact-page__container">

      <div class="contact-page__heading heading-large">
        Сайт онлайн-журнала Tabigat Media
        находится в разработке
      </div>

      <!--      <div class="contact-page__subtitle">-->
      <!--        Мы всегда готовы помочь и ответить на все ваши вопросы.-->
      <!--      </div>-->

      <div class="contact-page__external-box">
        <div class="contact-page__box contact-box">

          <div class="contact-box__title">
            Подпишитесь на рассылку и первым узнайте о запуске сайта <span>tabigat.media</span>
          </div>

          <form class="contact-box__form" @submit.prevent="subscribe">

            <div class="contact-box__input-box">
              <svg width="24" height="24">
                <use href="../assets/img/icons.svg#mail-2"></use>
              </svg>
              <input type="email" class="contact-box__input" placeholder="E-mail" required v-model="mail">
            </div>

            <button class="contact-box__btn" type="submit">Отправить</button>

          </form>

        </div>

        <div class="contact-page__social-box">

          <h4>Подпишитесь на нас в соцсетях:</h4>

          <div class="contact-page__socials">
            <a href="https://www.instagram.com/tabigat.media" target="_blank">
              <svg width="24" height="24" @click="">
                <use href="../assets/img/icons.svg#inst-brown"></use>
              </svg>
            </a>
            <a href="https://www.facebook.com/tabigat.media" target="_blank">
              <svg width="24" height="24" @click="">
                <use href="../assets/img/icons.svg#fb-brown"></use>
              </svg>
            </a>
            <a href="https://t.me/tabigatmedia" target="_blank">
              <svg width="24" height="24" @click="">
                <use href="../assets/img/icons.svg#tg-brown"></use>
              </svg>
            </a>
          </div>
        </div>
      </div>
    </div>

    <success-modal v-if="showModal"
                   @close="showModal = false"
                   :title="modalTitle"
                   :subtitle="modalSubtitle"
                   :type="modalType"></success-modal>

    <div class="mailing-section__loading-box" v-if="loadingState">
      <img src="../assets/img/icons/loader-bird.png" alt="" class="mailing-section__loader">
    </div>

  </div>
</template>

<script>
import SuccessModal from "../components/SuccessModal";

export default {
  data() {
    return {
      mail: '',
      modalType: false,
      showModal: false,
      modalTitle: '',
      modalSubtitle: '',
    };
  },
  components: {
    SuccessModal
  },
  computed: {
    loadingState() {
      return this.$store.getters.loadingState
    }
  },
  methods: {
    preventScroll(e) {
      e.preventDefault()
      e.stopPropagation()

      return false;
    },

    subscribe() {
      this.$store.commit('setLoadingState', true)

      document.querySelector('.ultra-main').addEventListener('wheel', this.preventScroll)

      this.$axios.post(process.env.API + 'mailings/subscribe/', {
        email: this.mail
      })
          .then(response => {
            this.$axios.post('https://api.sendpulse.com/addressbooks/' + localStorage.bookId + '/emails', {
              emails: [this.mail]
            }, {
              headers: {
                'Authorization': 'Bearer ' + localStorage.access_token
              }
            }).catch(e => {
              document.querySelector('.ultra-main').removeEventListener('wheel', this.preventScroll)
              this.$store.commit('setLoadingState', false)
            })
                .then(response => {
                  if (response.data.result === true) {
                    this.$store.commit('setLoadingState', false)
                    this.modalType = true
                    this.showModal = true
                    this.modalTitle = 'Спасибо!'
                    this.modalSubtitle = 'Вы успешно подписались на рассылку от Tabigat media'
                    this.mail = ''
                    document.querySelector('.ultra-main').removeEventListener('wheel', this.preventScroll)
                  } else {
                    this.$store.commit('setLoadingState', false)
                    this.modalType = false
                    this.showModal = true
                    this.modalTitle = 'Упс!'
                    this.modalSubtitle = 'Произошла ошибка, попробуйте снова'
                    this.mail = ''
                    document.querySelector('.ultra-main').removeEventListener('wheel', this.preventScroll)
                  }
                })
                .catch(e => console.log(e))
          })
          .catch(e => {
            if (e.response.data.errors.email[0] === "The email has already been taken.") {
              this.$store.commit('setLoadingState', false)
              this.modalType = false
              this.showModal = true
              this.modalTitle = 'Упс!'
              this.modalSubtitle = 'Произошла ошибка, данная почта уже подписана на рассылку'
              this.mail = ''
              document.querySelector('.ultra-main').removeEventListener('wheel', this.preventScroll)
            } else {
              this.$store.commit('setLoadingState', false)
              this.modalType = false
              this.showModal = true
              this.modalTitle = 'Упс!'
              this.modalSubtitle = 'Произошла ошибка, попробуйте снова'
              this.mail = ''
              document.querySelector('.ultra-main').removeEventListener('wheel', this.preventScroll)
            }
          })
    }
  },
  mounted() {
    this.$axios.post('https://api.sendpulse.com/oauth/access_token', {
      "grant_type": "client_credentials",
      "client_id": process.env.clientId,
      "client_secret": process.env.clientSecret
    })
        .then(response => {
          localStorage.access_token = response.data.access_token

          this.$axios.get('https://api.sendpulse.com/addressbooks?limit=10', {
            headers: {
              'Authorization': 'Bearer ' + response.data.access_token
            }
          })
              .then(response => {
                localStorage.bookId = response.data[0].id
              })
              .catch(e => console.log(e))
        })
        .catch(e => console.log(e))
  }
}
</script>

<style>

</style>
