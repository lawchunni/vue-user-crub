<template>
  <div>
    <div id="form-edit" class="modal is-active">

      <div @click="$emit('toggleDisplay', {type: type, mode: 'close', index: null} )" class="modal-background"></div>

      <div class="modal-card">
        <header class="modal-card-head">
          <p class="modal-card-title" v-text="details.title"></p>
          <button @click="$emit('toggleDisplay', {type: type, mode: 'close', index: null} )" class="delete" aria-label="close"></button>

        </header>
        <section class="modal-card-body">
          <form>

            <div class="field">
              <label class="label">Name</label>
              <div class="control">
                <input class="input" type="text" v-model="localUser.name" />
              </div>
            </div>
            <div class="field">
              <label class="label">Email</label>
              <div class="control">
                <input class="input" type="text" v-model="localUser.email" />
              </div>
            </div>
            <div class="field">
              <label class="label">Phone</label>
              <div class="control">
                <input class="input" type="text" v-model="localUser.phone" />
              </div>
            </div>
            <button @click.prevent="$emit('formSubmit', user)" class="button is-success" v-text="details.submit"></button>
            <button type="reset" @click="$emit('toggleDisplay', {type: type, mode: 'close', index: null} )" class="button">Cancel</button>
          </form>
          
        </section>

      </div>
    </div>
  </div>
</template>

<script>

export default {
  name: 'FormModal',
  props: {
    type: String,
    user: Object,
    
  },
  data() {
    return {
      localUser: this.user,
      details: Object,
      edit: {
        title: 'Edit a User',
        submit: 'Update',
      },
      create: {
        title: 'Create a New User',
        submit: 'Create',
      },
    }
  },
  created() {
    if (this.type == 'edit') {
      this.details = this.edit;
    } else {
      this.details = this.create;
    }
  }
}
</script>

<style scoped>
form button {
  margin: 10px 10px 0 0;
}
</style>