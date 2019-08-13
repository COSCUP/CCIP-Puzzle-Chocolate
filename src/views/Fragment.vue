<template>
  <div role="fragment">
    <h1 role="GameName">{{ $t('fragment') }}</h1>
    <template v-if="showScanner">
      <qrcode-reader
        :enable="showScanner"
        :noResult="true"
        title
        :subTitle="$t('scan_qrcode')"
        @success="onScanSuccess"
      ></qrcode-reader>
    </template>
    <template v-if="token !== null">
      <FragmentList :fragments="puzzleList" />
    </template>
  </div>
</template>

<script>
export default {
  name: 'Fragment',
  data () {
    return {
      mode: 'personal', // Or counter
      puzzles: ['整數(任意整數)', '整數(任意整數)', '整數(任意整數)', '整數(任意整數)', '整數(任意整數)', 'while'],
      puzzleCounter: [],
      token: null
    }
  },
  watch: {
    mode (newMode, previousMode) {
      if (newMode === 'counter') {
        this.puzzleCounter = [].concat(this.puzzles)
      } else {
        this.puzzleCounter = []
      }
    }
  },
  computed: {
    showScanner () {
      return this.token === null || this.mode === 'counter'
    },
    puzzleList () {
      const countingTarget = this.mode === 'personal' ? this.puzzles : this.mode === 'counter' ? this.puzzleCounter : []
      return countingTarget.reduce((statistics, puzzle) => {
        let target = statistics.find((puzzleStatistic) => puzzleStatistic.puzzle === puzzle)

        if (target === undefined) {
          statistics.push({ puzzle: puzzle, count: 1 })
        } else {
          target.count += 1
        }

        return statistics
      }, [])
    }
  },
  methods: {
    onScanSuccess (scanValue) {
      this.token = scanValue
    }
  }
}
</script>
