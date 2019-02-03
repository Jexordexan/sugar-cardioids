<template>
  <li class="heart" :class="color" :style="rotation">
    <input
      v-if="editing"
      class="message edit-input"
      type="text"
      :value="text"
      @keydown.enter="submit"
      @blur="submit"
    />
    <span v-else class="message" @click="$emit('edit')">
      {{ text }}
    </span>
    <div class="cardioid"></div>
  </li>
</template>

<script>
export default {
  props: {
    text: String,
    color: String,
    spin: Number,
    editing: Boolean
  },
  computed: {
    rotation() {
      return {
        transform: `rotate(${this.spin}deg)`
      };
    }
  },
  methods: {
    submit(event) {
      const text = event.target.value;
      this.$emit("change", text);
    }
  }
};
</script>

<style>
.edit-input {
  width: 50px;
  text-align: center;
}
</style>
