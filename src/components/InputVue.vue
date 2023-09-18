<script>
import { defineEmits, ref } from "vue";
export default {
  data() {
    return {
      text: ref(""),
    };
  },
  methods: {
    handleSubmit() {
      if (this.text != "") {
        //llama al emit
        this.$emit("onNewItem", this.text);
        this.text = "";
      }
    },
  },
};
</script>

<template>
  <form @submit.prevent="handleSubmit">
    <input type="text" v-model="text" />
    <button class="btn" type="submit">add</button>
  </form>
</template>

<style>
input{
    border: solid black 1px;
}
.btn{
    border-radius: 5px;
    background-color: green;
    color: black;
}

form{
    margin-bottom: 5px;
}
</style>
