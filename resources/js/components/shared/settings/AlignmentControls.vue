<template>
  <div class="alignment-controls flex justify-center">
    <ul
      class="
        flex
        items-center
        gap-x-3
        rounded
        border border-dashed
        p-6
        bg-white
      "
    >
      <li
        class="breakpoint-option px-1 cursor-pointer"
        :class="{
          'bg-grey-40  rounded':
            breakpoint === 'xs'
              ? selected === option.value
              : selected === `${breakpoint}:${option}`,
        }"
        @click="switchAlignment(option.value)"
        v-for="option in config.options"
        :key="`${option.value}-${selected}`"
      >
        <img class="w-6" :src="`/vendor/buildamic/img/${option.icon}.svg`" />
      </li>
    </ul>
  </div>
</template>

<script>
import { mapActions, mapGetters } from "vuex";
import { EvaIcon } from "vue-eva-icons";

export default {
  name: "alignment-controls",
  props: {
    value: String,
    options: {
      type: Array,
      default: () => [
        { value: "text-left", icon: "text-left" },
        { value: "text-center", icon: "text-center" },
        { value: "text-right", icon: "text-right" },
      ],
    },
  },
  data() {
    return {
      config: {
        options: this.options,
      },
    };
  },
  components: {
    EvaIcon,
  },
  computed: {
    ...mapGetters(["breakpoint"]),
    selected() {
      return this.value;
    },
  },
  methods: {
    switchAlignment(alignment) {
      if (this.breakpoint === "xs") {
        if (alignment === this.selected) {
          return this.$emit("input", "");
        }
        return this.$emit("input", alignment);
      }
      if (`${this.breakpoint}:${alignment}` === this.selected) {
        return this.$emit("input", "");
      }
      return this.$emit("input", `${this.breakpoint}:${alignment}`);
    },
  },
};
</script>
<style scoped></style>
