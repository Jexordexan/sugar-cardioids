<template>
  <li class="heart" :class="color" :style="rotation">
    <input
      v-if="editing"
      class="message edit-input"
      name="heart-message"
      type="text"
      :value="text"
      @keydown.enter="submit"
      @blur="submit"
    />
    <span v-else class="heart-message">{{ text }}</span>
    <div class="heart-shape"></div>
    <div v-if="!editing" class="heart-buttons">
      <button @click="$emit('eat')">
        eat it
      </button>
      <button @click="$emit('edit')">
        edit
      </button>
    </div>
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
      }
    }
  },
  methods: {
    submit(event) {
      const text = event.target.value
      this.$emit('change', text)
    }
  }
}
</script>

<style lang="less">
@import '../styles/styles';

@heart-radius: 30px;

.edit-input {
  width: 50px;
  text-align: center;
  font: 1em sans-serif;
}

.heart {
  @size: @heart-radius * 3;

  display: flex;
  position: relative;
  width: @size;
  height: @size;
  margin: 20px;
  justify-content: center;
  align-items: center;

  &:hover .heart-buttons,
  &:focus-within .heart-buttons {
    visibility: visible;
  }
}

button.reset {
  border: none;
  background: transparent;
}

.heart-buttons {
  position: absolute;
  z-index: 3;
  top: 0;
  left: 0;
  width: 100%;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  visibility: hidden;

  button {
    text-align: center;
    padding: 5px;
    margin: 2px;
    background-color: #FFF;
    border: 1px solid #F99;
    font: inherit;
    color: inherit;
    border-radius: 20px;
    box-shadow: 0 3px 6px rgba(100, 0, 10, .2), 0 10px 20px rgba(100, 0, 10, .2);
    
    &:hover {
      border-color: #F22;
    }
    
    &:active {
      background: #F99;
    }
    
    &:disabled {
      opacity: 0.5;
    }
  }
}

.heart-message {
  font: bold 16px ComicSansMS;
  text-transform: uppercase;
  text-shadow: 1px 1px 3px rgba(255, 255, 255, 1);
  color: darkred;
}

.heart-shape {
  @diam: 2 * @heart-radius;

  position: absolute;
  transform: rotate(-45deg);
  z-index: -1;

  &:before,
  &:after {
    display: block;
    content: '';
    background: @front;
    box-shadow: -7px 10px @side;
  }

  &:before {
    width: @diam;
    height: @heart-radius;
    border-radius: @heart-radius @heart-radius 0 0;
  }
  &:after {
    width: @diam + @heart-radius;
    height: @diam;
    border-radius: 0 @heart-radius @heart-radius 0;
  }
}
</style>
