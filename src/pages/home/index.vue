<template>
  <div
    id="main"
    :style="{ background: backgroundValue.value.toString() }"
    class="tw-flex tw-flex-col tw-flex-1 tw-mx-auto tw-w-full tw-p-4"
  >
    <template v-if="!isReadyStart && !isCountdown">
      <v-select
        v-model="backgroundValue"
        color="white"
        label="Set your backgroud color"
        :items="backgrounds"
        :item-value="backgroundValue.value"
        outlined
      >
        <template #selection="{ item }">
          <div>
            <span>{{ item.label }}</span>
          </div>
        </template>
        <template #item="{ item, on, attrs }">
          <div
            class="tw-border tw-border-white tw-h-4 tw-w-4 tw-rounded-full tw-mr-2"
            :style="{ background: item.value.toString() }"
          />
          <span v-bind="attrs" v-on="on">{{ item.label }}</span>
        </template>
        <template #prepend-item="{ item }">
          <div
            class="tw-border tw-border-white tw-h-4 tw-w-4"
            :class="`tw-bg-${item.value}`"
          />
        </template>
      </v-select>
      <v-text-field
        v-model="intro"
        color="white"
        height="2px"
        outlined
        clearable
        label="Intro text"
        required
      ></v-text-field>
      <v-text-field
        v-model="type"
        color="white"
        outlined
        clearable
        label="Your congratulatory type"
        required
      ></v-text-field>
      <v-text-field
        v-model="name"
        color="white"
        outlined
        clearable
        label="His/her name"
        required
      ></v-text-field>
      <div class="tw-flex tw-space-x-4">
        <v-text-field
          class="c-field"
          v-model="date"
          color="white"
          outlined
          clearable
          counter
          maxlength="10"
          label="Today"
          :rules="[rules.today]"
          required
        ></v-text-field>
        <v-text-field
          class="c-field"
          v-model="birthday"
          color="white"
          outlined
          clearable
          counter
          maxlength="10"
          label="His/her special date"
          :rules="[rules.special]"
          required
        ></v-text-field>
      </div>
      <v-text-field
        v-model="credit"
        color="white"
        outlined
        clearable
        label="Your name"
        required
      ></v-text-field>
      <v-text-field
        class="c-field"
        v-model="wish"
        color="white"
        rows="3"
        row-height="25"
        outlined
        clearable
        label="Your wish"
        counter
        maxlength="1000"
        required
      ></v-text-field>
      <ButtonStart @Event="ready()" :disabled="!isValid" />
    </template>
    <ButtonStart v-if="isReadyStart" @Event="start()" :text="'Start'" />
    <Countdown
      v-if="!isReadyStart && isCountdown"
      :intro="intro"
      :date="date"
      :type="type"
      :name="name"
      :credit="credit"
      :wish="wish"
      :birthday="birthday"
      @setClear="setClear()"
    />
    <div v-if="isBack" class="tw-mt-auto">
      <ButtonStart @Event="clear()" :text="'Back home'" />
    </div>
    <div
      class="tw-absolute tw-bottom-0 tw-right-0 tw-p-4 tw-text-xs tw-text-white tw-italic"
    >
      Since 2022 by Heyday Lê
    </div>
  </div>
</template>

<script>
import ButtonStart from "@/components/button-start";
import Countdown from "@/components/countdown";
import { backgrounds } from "@/enums";
export default {
  data() {
    return {
      date: "2022/1/1",
      birthday: "1999/1/1",
      name: "Lê Dương Hưng Thịnh",
      credit: "Heyday",
      wish: "Lorem Ipsum is simply dummy text of the printing and typesetting industry. Lorem Ipsum has been the industry's standard dummy text ever since the 1500s, when an unknown printer took a galley of type and scrambled it to make a type specimen book. It has survived not only five centuries, but also the leap into electronic typesetting, remaining essentially unchanged. It was popularised in the 1960s with the release of Letraset sheets containing Lorem Ipsum passages, and more recently with desktop publishing software like Aldus PageMaker including versions of Lorem Ipsum",
      backgroundValue: { label: "defaul", value: "#1fAc7a" },
      type: "Happi Birthday",
      intro: "Welcome to the playground",

      backgrounds,
      query: "",
      currentUrl: "",

      isReadyStart: false,
      isCountdown: false,
      isBack: false,
      isCopy: false,

      rules: {
        wish: (len) => (v) =>
          (!!v || "").length >= len ||
          `Invalid character length, required ${len}`,
        today: (v) =>
          ((!!v || "") && this.dateFormat > this.birthdayFormat) ||
          "this day must be bigger than the special date",
        special: (v) =>
          ((!!v || "") && this.dateFormat > this.birthdayFormat) ||
          "this special date must be less than today",
      },
    };
  },
  computed: {
    dateFormat() {
      let d = new Date(this.date);
      return d.getFullYear();
    },
    birthdayFormat() {
      let d = new Date(this.birthday);
      return d.getFullYear();
    },
    isValid() {
      return (
        !!this.type &&
        !!this.date &&
        !!this.birthday &&
        !!this.name &&
        !!this.credit &&
        !!this.wish &&
        !!this.backgroundValue &&
        this.birthdayFormat < this.dateFormat
      );
    },
  },
  components: {
    ButtonStart,
    Countdown,
  },
  created() {
    this.setFromQuery();
  },
  mounted() {
    this.currentUrl = window.location.hostname;
  },
  methods: {
    ready() {
      this.isReadyStart = true;
      this.isCopy = true;
      this.query = {
        name: this.name,
        date: this.date,
        birthday: this.birthday,
        credit: this.credit,
        wish: this.wish,
        backgroundValue: this.backgroundValue.value,
        type: this.type,
        intro: this.intro,
      };
      this.$router.replace({
        path: "/",
        query: this.query,
      });
    },
    start() {
      this.isReadyStart = false;
      this.isCountdown = true;
    },
    setClear() {
      this.isBack = true;
    },
    clear() {
      this.isCountdown = false;
      this.isBack = false;
    },
    setFromQuery() {
      if (
        this.$route.query.name &&
        this.$route.query.date &&
        this.$route.query.birthday &&
        this.$route.query.credit &&
        this.$route.query.wish &&
        this.$route.query.backgroundValue &&
        this.$route.query.type &&
        this.$route.query.intro
      ) {
        this.name = this.$route.query.name;
        this.date = this.$route.query.date;
        this.birthday = this.$route.query.birthday;
        this.credit = this.$route.query.credit;
        this.wish = this.$route.query.wish;
        this.backgroundValue.value = this.$route.query.backgroundValue;
        this.type = this.$route.query.type;
        this.intro = this.$route.query.intro;
        this.isReadyStart = true;
        this.copyURL();
      }
    },
    copyURL() {
      const el = document.createElement("textarea");
      el.value = window.location.href;
      el.setAttribute("readonly", "");
      el.style.position = "absolute";
      el.style.left = "-9999px";
      document.body.appendChild(el);
      const selected =
        document.getSelection().rangeCount > 0
          ? document.getSelection().getRangeAt(0)
          : false;
      el.select();
      document.execCommand("copy");
      document.body.removeChild(el);
      if (selected) {
        document.getSelection().removeAllRanges();
        document.getSelection().addRange(selected);
      }
    },
  },
};
</script>

<style lang="scss">
::-webkit-scrollbar {
  width: 5px;
}
::-webkit-scrollbar-track {
  background: #b6b6b600;
}
::-webkit-scrollbar-thumb {
  background: rgb(255, 255, 255);
  border-radius: 5px;
}
#main {
  height: 100vh;
  overflow: hidden;
}
.v-input__slot {
  max-height: 14px;
}
.v-text-field__details {
  display: none;
}
.c-field {
  .v-text-field__details {
    .v-messages {
      display: none;
    }
    .v-counter {
      margin-left: auto;
    }
    display: inline;
  }
}
</style>
