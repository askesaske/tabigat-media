<!--<template>-->
<!--  <section class="mailing-section" id="mail-box">-->
<!--    <div class="mailing-section__container">-->
<!--      <div class="mailing-section__info">-->
<!--        <div class="mailing-section__title">-->
<!--          Подписаться на рассылку-->
<!--        </div>-->

<!--        <div class="mailing-section__subtitle">-->
<!--          Подпишись на рассылку от Tabigat media, чтобы читать самые свежие и интересные статьи первыми-->
<!--        </div>-->
<!--      </div>-->

<!--      <form class="mailing-section__active-box" @submit.prevent="subscribe">-->
<!--        <div class="mailing-section__input-box">-->
<!--          <svg width="24" height="24">-->
<!--            <use href="../assets/img/icons.svg#mail"></use>-->
<!--          </svg>-->

<!--          <input type="email"-->
<!--                 class="mailing-section__input"-->
<!--                 name="email"-->
<!--                 required-->
<!--                 placeholder="Введите ваш email ( example@gmail.com )"-->
<!--                 v-model="mail">-->
<!--        </div>-->

<!--        <button class="mailing-section__btn">-->
<!--          Подписаться-->
<!--        </button>-->
<!--      </form>-->

<!--      <success-modal v-if="showModal"-->
<!--                     @close="showModal = false"-->
<!--                     :title="modalTitle"-->
<!--                     :subtitle="modalSubtitle"-->
<!--                     :type="modalType"></success-modal>-->

<!--      <div class="mailing-section__loading-box" v-if="loadingState">-->
<!--        <img src="../assets/img/icons/loader-bird.png" alt="" class="mailing-section__loader">-->
<!--      </div>-->
<!--    </div>-->
<!--  </section>-->
<!--</template>-->

<!--<script>-->
<!--import SuccessModal from "./SuccessModal";-->

<!--export default {-->
<!--  components: {-->
<!--    SuccessModal-->
<!--  },-->
<!--  data() {-->
<!--    return {-->
<!--      mail: '',-->
<!--      modalType: false,-->
<!--      showModal: false,-->
<!--      modalTitle: '',-->
<!--      modalSubtitle: '',-->
<!--    }-->
<!--  },-->
<!--  computed: {-->
<!--    loadingState() {-->
<!--      return this.$store.getters.loadingState-->
<!--    }-->
<!--  },-->
<!--  methods: {-->
<!--    preventScroll(e) {-->
<!--      e.preventDefault()-->
<!--      e.stopPropagation()-->

<!--      return false;-->
<!--    },-->

<!--    subscribe() {-->
<!--      this.$store.commit('setLoadingState', true)-->

<!--      document.querySelector('.ultra-main').addEventListener('wheel', this.preventScroll)-->

<!--      this.$axios.post(process.env.API + 'mailings/subscribe/', {-->
<!--        email: this.mail-->
<!--      })-->
<!--          .then(response => {-->
<!--            this.$axios.post('https://api.sendpulse.com/addressbooks/' + localStorage.bookId + '/emails', {-->
<!--              emails: [this.mail]-->
<!--            }, {-->
<!--              headers: {-->
<!--                'Authorization': 'Bearer ' + localStorage.access_token-->
<!--              }-->
<!--            }).catch(e => {-->
<!--              document.querySelector('.ultra-main').removeEventListener('wheel', this.preventScroll)-->
<!--              this.$store.commit('setLoadingState', false)-->
<!--            })-->
<!--                .then(response => {-->
<!--                  if (response.data.result === true) {-->
<!--                    this.$store.commit('setLoadingState', false)-->
<!--                    this.modalType = true-->
<!--                    this.showModal = true-->
<!--                    this.modalTitle = 'Спасибо!'-->
<!--                    this.modalSubtitle = 'Вы успешно подписались на рассылку от Tabigat media'-->
<!--                    this.mail = ''-->
<!--                    document.querySelector('.ultra-main').removeEventListener('wheel', this.preventScroll)-->
<!--                  } else {-->
<!--                    this.$store.commit('setLoadingState', false)-->
<!--                    this.modalType = false-->
<!--                    this.showModal = true-->
<!--                    this.modalTitle = 'Упс!'-->
<!--                    this.modalSubtitle = 'Произошла ошибка, попробуйте снова'-->
<!--                    this.mail = ''-->
<!--                    document.querySelector('.ultra-main').removeEventListener('wheel', this.preventScroll)-->
<!--                  }-->
<!--                })-->
<!--                .catch(e => console.log(e))-->
<!--          })-->
<!--          .catch(e => {-->
<!--            if (e.response.data.errors.email[0] === "The email has already been taken.") {-->
<!--              this.$store.commit('setLoadingState', false)-->
<!--              this.modalType = false-->
<!--              this.showModal = true-->
<!--              this.modalTitle = 'Упс!'-->
<!--              this.modalSubtitle = 'Произошла ошибка, данная почта уже подписана на рассылку'-->
<!--              this.mail = ''-->
<!--              document.querySelector('.ultra-main').removeEventListener('wheel', this.preventScroll)-->
<!--            } else {-->
<!--              this.$store.commit('setLoadingState', false)-->
<!--              this.modalType = false-->
<!--              this.showModal = true-->
<!--              this.modalTitle = 'Упс!'-->
<!--              this.modalSubtitle = 'Произошла ошибка, попробуйте снова'-->
<!--              this.mail = ''-->
<!--              document.querySelector('.ultra-main').removeEventListener('wheel', this.preventScroll)-->
<!--            }-->
<!--          })-->
<!--    }-->
<!--  },-->
<!--  mounted() {-->
<!--    this.$axios.post('https://api.sendpulse.com/oauth/access_token', {-->
<!--      "grant_type": "client_credentials",-->
<!--      "client_id": process.env.clientId,-->
<!--      "client_secret": process.env.clientSecret-->
<!--    })-->
<!--        .then(response => {-->
<!--          localStorage.access_token = response.data.access_token-->

<!--          this.$axios.get('https://api.sendpulse.com/addressbooks?limit=10', {-->
<!--            headers: {-->
<!--              'Authorization': 'Bearer ' + response.data.access_token-->
<!--            }-->
<!--          })-->
<!--              .then(response => {-->
<!--                localStorage.bookId = response.data[0].id-->
<!--              })-->
<!--              .catch(e => console.log(e))-->
<!--        })-->
<!--        .catch(e => console.log(e))-->
<!--  }-->
<!--}-->
<!--</script>-->