<template>
  <section>
    <div v-if="!nameEntered">
      <b-form-input required @keyup.enter="submitName" v-model="name" placeholder="Enter your name"></b-form-input>
    </div>
    <div v-else>
      <b-table striped hover :items="items"></b-table>
    </div>
  </section>
</template>

<script>
export default {
  props: {
    result: Number,
  },
  data() {
    return {
      name: null,
      nameEntered: false,
      items: [
        { result: 17, first_name: "Tigran" },
        { result: 14, first_name: "Zara" },
        { result: 12, first_name: "Vardan" },
        { result: 8, first_name: "Maria" },
      ],
    };
  },
  methods: {
    submitName() {
      this.nameEntered = true;
      let newResult = new Object();
      newResult.result = this.result * 20;
      newResult.first_name = this.name;
      this.items.push(newResult);
      this.items = this.items.sort((a, b) => (a.result > b.result ? -1 : 1));
    },
  },
};
</script>