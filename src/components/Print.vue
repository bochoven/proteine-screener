<template>
  <b-container class="home">
    <b-row class="d-print-none pt-2">
      <b-col>
        <b-button variant="outline-secondary" onclick="window.print()">
            <font-awesome-icon icon="print"/> {{ $t('print.print') }}
          </b-button>
          <a onclick="this.href='data:text/html;charset=UTF-8,'+encodeURIComponent(document.documentElement.outerHTML)" 
            href="#"
            class="btn btn-outline-secondary"
            download="proteinscreener.html"
          >
            <font-awesome-icon icon="download"/> {{ $t('print.download') }}
          </a>                    
      </b-col>
    </b-row>   
    <b-row class="pt-2">
      <b-col cols="12" md="8">
        <h2>{{ $t('print.title') }}</h2>
        <p>{{ getDateString }}</p>
        <p v-for="item in renderQuizResults" :key="item.id">
            <strong>{{ $t('question_number', {number: item.number}) }}:</strong>
              {{ $t(item.id + '.text') }} <br>
              {{ $t('print.your_answer') }}:
              <template v-if="item.answer != undefined">
              {{ $t(item.answer.text, {count: item.answer.count, fraction: item.answer.fraction}, item.answer.count) }}
              </template>
        </p>
        <a
          class="btn d-print-none btn-info btn-lg"
          @click="$router.go(-1)">{{ $t('nav.back') }}
        </a>
      </b-col>
      <b-col cols="12" md="4" class="d-none d-md-block">
        <client-results-block/>
        <protein-target-client/>
        <img class="img-fluid" src="../assets/egg_measuring_tape.jpg" />
      </b-col>
    </b-row>
    <b-row class="pt-2">
      <b-col cols="12" md="8">
        <b-row>
          <b-col cols="6">
              <img class="img-fluid" src="../assets/VUlogo.svg" />
          </b-col>
          <b-col cols="6">
              <img class="img-fluid" src="../assets/promiss_logo.svg" />
          </b-col>
        </b-row>
      </b-col>
    </b-row>
  </b-container>
</template>

<script>
import { mapGetters } from 'vuex'
import FontAwesomeIcon from '@fortawesome/vue-fontawesome'
import router from '../router'
import ClientResultsBlock from './results/ClientResultsBlock'
import ProteinTargetClient from './results/ProteinTargetClient'

export default {
  name: 'Print',
  components: {
    FontAwesomeIcon,
    ClientResultsBlock,
    ProteinTargetClient
  },
  computed: {
    ...mapGetters({
      renderQuizResults: 'quiz/renderQuizResults'
    }),
    getDateString () {
      let event = new Date()
      let options = {weekday: 'long', year: 'numeric', month: 'long', day: 'numeric'}
      let locale = router.app.$i18n.locale()
      return event.toLocaleDateString(locale, options)
    }
  }
}
</script>
