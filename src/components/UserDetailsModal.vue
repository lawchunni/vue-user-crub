<template>
  <div>
		<div id="user-details" class="modal is-active">

      <div @click="$emit('toggleDisplay', {user: localUser, display: false} )" class="modal-background"></div>

      <div class="modal-card">
        <header class="modal-card-head">
          <p class="modal-card-title">User Details - <span v-text="localUser.name"></span></p>
          <button @click="$emit('toggleDisplay', {user: localUser, display: false} )" class="delete" aria-label="close"></button>
        </header>
        <section class="modal-card-body">
					<ul>
						<li><strong>ID: </strong><span v-text="localUser.id"></span></li>
						<li><strong>Name: </strong><span v-text="localUser.name"></span></li>
						<li><strong>Email: </strong><span v-text="localUser.email"></span></li>
						<li><strong>Phone: </strong><span v-text="localUser.phone"></span></li>
						<li><strong>Created At: </strong><span v-text="localUser.created_at"></span></li>
						<li><strong>Updated At: </strong><span v-text="localUser.updated_at"></span></li>
					</ul>
					<button @click="$emit('toggleDisplay', {user: localUser, display: false} )" class="button">Cancel</button>
        </section>
				
      </div>
    </div>
	</div>
</template>

<script>
export default {
	name: 'UserDetailsModal',
	props: {
		user: Object,
	},
	data() {
		return {
			localUser: this.user,
		}
	},
}
</script>

<style scoped>
ul li {
	line-height: 2em;
}
button {
	margin-top: 10px;
}
</style>