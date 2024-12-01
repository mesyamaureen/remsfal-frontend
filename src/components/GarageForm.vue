<template>
    <form @submit.prevent="submitForm">
      <label>Title:</label>
      <input v-model="form.title" required />
      <label>Location:</label>
      <input v-model="form.location" required />
      <label>Description:</label>
      <textarea v-model="form.description"></textarea>
      <label>Usable Space:</label>
      <input v-model="form.usableSpace" type="number" required />
      <label>Rent:</label>
      <input v-model="form.rent" type="number" required />
      <button type="submit">Save</button>
      <button type="button" @click="$emit('cancel')">Cancel</button>
    </form>
  </template>
  
  <script lang="ts">
  export default {
    props: ['garage'],
    data() {
      return {
        form: {
          title: '',
          location: '',
          description: '',
          usableSpace: null,
          rent: null,
        },
      };
    },
    watch: {
      garage: {
        immediate: true,
        handler(newVal) {
          if (newVal) this.form = { ...newVal };
        },
      },
    },
    methods: {
      submitForm() {
        this.$emit('submit', this.form);
      },
    },
  };
  </script>
  