<template>
    <div>
        <b-alert :show="noDoctorFound" variant="warning">
            {{ $t('Please, check that doctor was assigned to this account') }}
        </b-alert>
        <div v-if="doctor" class="card card-about">
            <div class="card-block m-2">
                <h6 class="mb-2">{{ $t('My Data') }}
                    <small> · <router-link :to="'/profile'">{{ $t('Edit') }}</router-link></small>
                </h6>
                <div class="row mb-2">
                    <div class="col-12">
                        <b>{{ $t('City') }}</b>
                        <br><span class="ml-1">{{ doctor.city ? doctor.city : $t('Not set') }}</span>
                    </div>
                </div>
                <div class="row mb-2">
                    <div class="col-12">
                        <b>{{ $t('Phone') }}</b>
                        <br><span class="ml-1">{{ doctor.phones ? doctor.phones : $t('Not set') }}</span>
                    </div>
                </div>
                <div class="row mb-2">
                    <div class="col-12">
                        <b :title="$t('Medical Board Number')">{{ $t('MBN') }}</b>
                        <br><span class="ml-1">{{ doctor.medical_board_num ? doctor.medical_board_num : $t('Not set') }}</span>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>
<style lang="scss">
    @import "../../sass/variables";
    .card-about {
        font-size: $font-size-sm;
        .val {
            padding-left: .5rem;
        }
    }
</style>
<script>
  import DoctorProvider from '../../providers/doctor.vue'

  export default {
    inject: ['loadingBarWrapper'],
    notifications: {
      showGowl: {
        type: 'error'
      }
    },
    created: function () {
      this.fetchData()
    },
    data () {
      return {
        doctor: null,
        noDoctorFound: false
      }
    },
    methods: {
      fetchData () {
        this.loadingBarWrapper.ref.start()
        this.doctor = null
        DoctorProvider.get().then(
          (response) => {
            this.doctor = response.data.data
            this.loadingBarWrapper.ref.done()
          },
          (err) => {
            this.showGowl({
              title: this.$t('Loading Error'),
              message: this.$t('Server error'),
              consoleMessage: err
            })
            this.loadingBarWrapper.ref.fail()
            this.noDoctorFound = true
          }
        )
      }
    }
  }
</script>
