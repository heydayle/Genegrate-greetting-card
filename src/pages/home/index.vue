<template>
  <div
    id="main"
    :class="!!backgroundValue.value ? backgroundValue.value.toString() : 'tw-bg-green-500'"
    class="tw-flex tw-flex-col tw-flex-1 tw-mx-auto tw-w-full tw-p-4"
  >
    <template v-if="!isCoutdown">
        <v-select
        v-model="backgroundValue"
        label="Your backgroud color"
        :items="backgrounds"
          :item-value="backgroundValue.value"
          outlined
        >
        <template #selection="{ item, }">
            <div>
              <span :class="`tw-text-${item.value}`">{{ item.label }}</span>
            </div>
          </template>
        <template #item="{item, on, attrs}" >
            <span v-bind="attrs" v-on="on">{{item.label}}</span>
        </template>
        <template #prepend-item="{item}">
            <div class="tw-border tw-border-white tw-h-4 tw-w-4"
            :class="`tw-bg-${item.value}`"/>
          </template>
        </v-select>
      <v-text-field v-model="type" color="white" outlined label="Your type" required></v-text-field>
      <v-text-field v-model="name" color="white" outlined label="His/her name" required></v-text-field>
      <v-text-field v-model="date" color="white" outlined label="Today" required></v-text-field>
      <v-text-field v-model="birthday" color="white" outlined label="His/her birthday" required></v-text-field>
      <v-text-field v-model="wish" color="white" outlined label="Your wish" required></v-text-field>
      <ButtonStart @Event="start()" :disabled="!isValid"/>
    </template>
    <Countdown
      v-else
      :date="date"
      :type="type"
      :name="name"
      :credit="credit"
      :wish="wish"
      :birthday="birthday"
      @setClear="setClear()"
    />
    <ButtonStart v-if="isBack" @Event="clear()" :text="'Back home'"/>
  </div>
</template>

<script>
import ButtonStart from "@/components/button-start";
import Countdown from "@/components/countdown";
import { backgrounds } from "@/enums"
export default {
  data() {
    return {
      date: "2022/1/1",
      birthday: "1999/1/1",
      name: "Lê Dương Hưng Thịnh",
      credit: "Heyday",
      wish: "Lorem Ipsum is simply dummy text of the printing and typesetting industry. Lorem Ipsum has been the industry's standard dummy text ever since the 1500s, when an unknown printer took a galley of type and scrambled it to make a type specimen book. It has survived not only five centuries, but also the leap into electronic typesetting, remaining essentially unchanged. It was popularised in the 1960s with the release of Letraset sheets containing Lorem Ipsum passages, and more recently with desktop publishing software like Aldus PageMaker including versions of Lorem Ipsum",
      backgroundValue: { label: "defaul", value: "green-500" },
      type: "Happi Birthday",


      backgrounds,

      isCoutdown: false,
      isBack: false,
    };
  },
  computed: {
      isValid() {
          return !!this.type && !!this.date && !!this.birthday && !!this.name && !!this.credit && !!this.wish && !!this.backgroundValue;
      }
  },
  components: {
    ButtonStart,
    Countdown,
  },
  methods: {
    start() {
      this.isCoutdown = true;
    },
    setClear() {
      this.isBack = true;
    },
    clear() {
      this.isCoutdown = false;
      this.isBack = false;
    }
  },
};
</script>

<style lang="scss">
#main {
  height: 100vh;
}
</style>
