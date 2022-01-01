<template>
  <div class="tw-text-5xl tw-font-light tw-text-white tw-text-center">
    <div v-if="isComing" class="tw-duration-500 tw-animate-fadeInTop100">
      To day <span class="tw-font-bold">{{ getDay + "/" + getMonth + "/" + getYear }}</span> is a special day!
    </div>
    <div v-if="isReady" class="tw-duration-500 tw-animate-fadeInTop100">
      Let's count down together
    </div>
    <div class="tw-h-24 tw-mb-6">
      <div v-if="year === getYearBirtday" class="tw-animate-fadeInTop100">
          <div class="tw-font-bold">{{type}}</div>
        <div class="tw-font-light tw-mt-2">{{ name }}</div>
      </div>
    </div>
    <div
      v-if="!isCountdown && !isComing && !isReady"
      class="tw-duration-1000 tw-animate-fadeInTop100"
    >
      <span class="tw-duration-300 tw-animate-fadeInTop100">{{ getDay }}</span
      >/
      <span class="tw-duration-600 tw-animate-fadeInbottom">{{ getMonth }}</span
      >/
      <span class="tw-duration-500 tw-animate-bounceIn">{{ getYear }}</span>
    </div>
    
    <div v-if="isCountdown">
      <span>{{ day }}</span>/
      <span>{{ month }}</span>/
      <span>{{ year }}</span>
    </div>
    <div class="tw-h-10 tw-mt-2">
      <div v-if="year === getYearBirtday" class="tw-animate-fadeInTop100">
        <div
          v-if="isWish"
          class="tw-animate-fadeInTop100 tw-text-xl tw-font-semibold"
        >
          <div class="tw-font-light">"{{wish}}"</div>
          <div>by <span class="tw-italic">{{credit}}</span></div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      dateTime: "",
      day: "",
      month: "",
      year: "",

      isCountdown: false,
      isComing: false,
      isWish: false,
      isReady: false,

      secondsSlowMotion: 500,
    };
  },
  props: {
    date: {
      type: String,
      default: "",
    },
    birthday: {
      type: String,
      default: "",
    },
    name: {
      type: String,
      default: "",
    },
    wish: {
      type: String,
      default: "",
    },
    credit: {
      type: String,
      default: "",
    },
    type: {
      type: String,
      default: "",
    }
  },
  computed: {
    getYear() {
      const d = new Date(this.dateTime);
      return d.getFullYear();
    },
    getMonth() {
      const d = new Date(this.dateTime);
      return this.formartDate(d.getMonth() + 1);
    },
    getDay() {
      const d = new Date(this.dateTime);
      return this.formartDate(d.getDate());
    },
    getYearBirtday() {
      const d = new Date(this.birthday);
      return d.getFullYear();
    },
  },
  created() {
    this.fetchDate();
  },
  methods: {
    clear() {
        this.$emit('setClear')
    },
    fetchDate() {
      this.isComing = true;
      this.dateTime = this.date;
      this.countdownEvent();
    },
    countdownEvent() {
      setTimeout(() => {
        this.isComing = false;
        this.isReady = true;
        setTimeout(() => {
          this.isReady = false;
          setTimeout(() => {
            this.countdown();
          }, 2000);
        }, 3000);
      }, 3000);
    },
    countdown() {
      this.isCountdown = true;
      this.day = this.getDay;
      this.month = this.getMonth;
      this.year = this.getYear;

      const d = new Date(this.birthday);

      setInterval(() => {
        if (this.year > d.getFullYear()) {
          this.year -= 1;
        }
      }, this.secondsSlowMotion);
      setInterval(() => {
        if (this.year > d.getFullYear()) {
          this.month = this.formartDate(
            Math.floor(Math.random() * (12 - 1 + 1) + 1)
          );
          this.day = this.formartDate(
            Math.floor(Math.random() * (31 - 1 + 1) + 1)
          );
        } else if (this.year === d.getFullYear()) {
          this.day = this.formartDate(d.getDate());
          this.month = this.formartDate(d.getMonth() + 1);
          this.year = d.getFullYear();
          this.setIsWish();
        }
      }, 10);
    },
    setIsWish() {
      setTimeout(() => {
        this.isWish = true;
        this.clear();
      }, 1000);
    },
    formartDate(number) {
      if (number < 10) {
        return "0" + number;
      } else return number;
    },
  },
};
</script>
