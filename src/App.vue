<template>
  <div id="app">
    <button @click="add_sat_and_sun_of_year">載入週六日</button>
    <button @click="remove_sat_and_sun_of_year">移除週六日</button>
    <year-calendar
      v-model="year"
      :activeDates.sync="activeDates"
      @toggleDate="toggleDate"
    ></year-calendar>
  </div>
</template>

<script>
import YearCalendar from './components/YearCalendar.vue'
import dayjs from 'dayjs'

export default {
  name: 'app',
  components: {
    YearCalendar
  },
  data () {
    return {
      year: 2019,
      activeDates: ['2019-01-01', '2019-01-02', '2019-01-03']
    }
  },
  methods: {
    toggleDate (dateInfo) {
    },
    add_sat_and_sun_of_year () {
      let theDate = dayjs(`${this.year}-01-01`)

      while (theDate.diff(theDate.endOf('year'), 'day') !== 0) {
        if (theDate.day() === 6 || theDate.day() === 0) {
          // 將週六或週日加入 activeDates 中
          this.activeDates.push(theDate.format('YYYY-MM-DD'))
        }
        theDate = theDate.add(1, 'day')
      }

      // remove duplicate key
      this.activeDates = this.activeDates.filter(function (item, pos, self) {
        return self.indexOf(item) === pos
      }).sort()
    },
    remove_sat_and_sun_of_year () {
      for (let i = this.activeDates.length - 1; i >= 0; i--) {
        let date = this.activeDates[i]
        if (dayjs(date).year() === this.year && (dayjs(date).day() === 6 || dayjs(date).day() === 0)) {
          this.activeDates.splice(i, 1)
        }
      }
    }
  }
}
</script>
<style lang="stylus">
*
  margin 0
  padding 0
  box-sizing border-box
#app
  padding 60px
  background-color #eaeaea
</style>
