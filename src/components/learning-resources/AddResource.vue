<template>
  <base-dialog v-if="inputIsInvalid" @close="confirmError" title="Invalid input">
      <template #default>
        <p>Unfortunatly, at least one input value is invalid.</p>
        <p>Please check all input and make sure you enter at least on character</p>
      </template>
      <template #actions>
        <base-button @click="confirmError">Okay</base-button>
      </template>
  </base-dialog>
  <base-card>
    <h2>Add Resource</h2>
    <form @submit.prevent="submitData">
      <div class="form-control">
        <label for="title">Title</label>
        <input type="text" name="title" id="title" ref="titleInput">
      </div>
      <div class="form-control">
        <label for="description">Description</label>
        <textarea name="desciption" id="description" rows="3" ref="desInput"></textarea>
      </div>
      <div class="form-control">
        <label for="link">Link</label>
        <input type="url" name="link" id="link" ref="linkInput">
      </div>
      <div class="form-control">
        <base-button type="submit"> Submit </base-button>
      </div>
    </form>
  </base-card>
</template>

<script>
import BaseButton from '../UI/BaseButton.vue';
export default {
  components: { BaseButton },
  data(){
    return {
      inputIsInvalid: false,
    }
  },
  inject: ['addResource'],
  methods: {
    submitData(){
      const title = this.$refs.titleInput.value;
      const link = this.$refs.linkInput.value;
      const description = this.$refs.desInput.value;

      if(title.trim() === '' || link.trim() === '' || description.trim() === ''){
        this.inputIsInvalid=true;
        return;
      }

      this.$refs.titleInput.value = '';
      this.$refs.linkInput.value = '';
      this.$refs.desInput.value = '';

      this.addResource(title, description, link);
    },
    confirmError(){
      this.inputIsInvalid = false;
    }
  },

}
</script>

<style scoped>
label {
  font-weight: bold;
  display: block;
  margin-bottom: 0.5rem;
}

input,
textarea {
  display: block;
  width: 100%;
  font: inherit;
  padding: 0.15rem;
  border: 1px solid #ccc;
}

input:focus,
textarea:focus {
  outline: none;
  border-color: #3a0061;
  background-color: #f7ebff;
}

.form-control {
  margin: 1rem 0;
}
</style>