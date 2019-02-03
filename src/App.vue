<template>
  <div id="app">
    <h1>Compressed Sugar Cardioids</h1>
    <form class="form" @submit.prevent="submit">
      <input class="heart-input" type="text" v-model="heartText" />
      <br />
      <label v-for="color in colorOptions" :key="color" :title="color">
        <div class="swatch" :class="color">
          <input
            v-model="heartColor"
            type="radio"
            name="color"
            :value="color"
          />
        </div>
      </label>
    </form>

    <ul class="heart-list">
      <Heart
        v-for="heart in hearts"
        :key="heart.id"
        :text="heart.text"
        :color="heart.color"
        :spin="heart.spin"
        :editing="editId === heart.id"
        @edit="startEdit(heart.id)"
        @change="submitEdit(heart, $event)"
        @eat="eat(heart.id)"
      />
    </ul>
  </div>
</template>

<script>
import Heart from "./components/Heart.vue";
let _idCounter = 0;
const colorOptions = ["orange", "yellow", "green", "blue", "purple", "pink"];

function createHeart(text, color) {
  if (color === "random") {
    color = colorOptions[Math.floor(Math.random() * colorOptions.length)];
  }

  return {
    id: _idCounter++,
    text,
    color,
    spin: 10 - Math.random() * 20
  };
}

export default {
  name: "app",
  components: {
    Heart
  },
  data() {
    return {
      heartText: "",
      heartColor: "orange",
      colorOptions: ["random", ...colorOptions],
      editId: null,
      hearts: [
        createHeart("I", "orange"),
        createHeart("Love", "green"),
        createHeart("Vue", "blue")
      ]
    };
  },
  methods: {
    submit() {
      if (!this.heartText) {
        return;
      }

      this.createHeart(this.heartText, this.heartColor);
      this.heartText = "";
    },
    eat(id) {
      const index = this.hearts.findIndex(h => h.id === id);
      this.hearts.splice(index, 1);
    },
    createHeart(text, color) {
      this.hearts.push(createHeart(text, color));
    },
    startEdit(id) {
      this.editId = id;
    },
    submitEdit(heart, text) {
      heart.text = text;
      this.editId = null;
    }
  }
};
</script>
