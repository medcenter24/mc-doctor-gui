<template>
    <div>
        <v-select
                :clearSearchOnSelect="true"
                :debounce="250"
                :on-search="onSearch"
                :value.sync="selected"
                :options="options"
                :on-change="onChange"
                label="title"
                :placeholder="$t('Surveys')"
        ></v-select>
    </div>
</template>
<script>
  import _ from 'lodash'
  import vSelect from 'vue-select'
  import SurveyProvider from '../../providers/survey.vue'

  export default {
    inject: ['loadingBarWrapper'],
    components: {
      vSelect
    },
    props: {
      excluded: {
        type: Array,
        default: () => []
      }
    },
    data () {
      return {
        selected: null,
        options: [],
        surveys: []
      }
    },
    created: function () {
      this.fetchData()
    },
    notifications: {
      showHttpError: {type: 'error'}
    },
    methods: {
      onSearch (search, loading) {
        console.log('surveySelector should be improved to except code duplicate')
        this.loadingBarWrapper.ref.start()
        loading(true)
        SurveyProvider.get().then(
          (response) => {
            this.surveys = response.data.data
            loading(false)
            this.loadingBarWrapper.ref.done()
          },
          (err) => {
            this.showHttpError({
              title: this.$t('API Error'),
              message: this.$t('Can\'t get Surveys'),
              consoleMessage: err.message
            })
            this.loadingBarWrapper.ref.fail()
            loading(false)
          }
        )
      },
      fetchData () {
        this.loadingBarWrapper.ref.start()
        SurveyProvider.get().then(
          (response) => {
            this.options = response.data.data
            this.loadingBarWrapper.ref.done()
          },
          (err) => {
            this.showHttpError({
              title: this.$t('API Error'),
              message: this.$t('Can\'t get Surveys'),
              consoleMessage: err.message
            })
            this.loadingBarWrapper.ref.fail()
          }
        )
      },
      onChange (survey) {
        this.surveys = _.filter(this.surveys, (iSurvey) => {
          return iSurvey.id === survey.id
        })
        this.$events.fire('survey-selector:change', survey, event)
      }
    }
  }
</script>
