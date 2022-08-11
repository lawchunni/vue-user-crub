<template>
  <tr>
		<td @click="$emit('toggleDetailsDisplay', {user: localUser, display: true} )" v-text="localUser.id"></td>
		<td @click="$emit('toggleDetailsDisplay', {user: localUser, display: true} )" v-text="localUser.name"></td>
		<td @click="$emit('toggleDetailsDisplay', {user: localUser, display: true} )" v-text="localUser.email"></td>
		<td @click="$emit('toggleDetailsDisplay', {user: localUser, display: true} )" v-text="localUser.phone"></td>
		<td>
			<button @click="$emit('toggleFormDisplay', {type: 'edit', mode: 'open', index: localIndex} )" class="button is-primary is-light">Edit</button>
			<button @click="$emit('deleteUser', localUser)" class="button is-danger is-light">Delete</button>
		</td>
	</tr>
</template>

<script>
export default {
	name: 'UserListDetails',
	props: {
		index: Number,
		user: Object,
	},
	data() {
		return {
			localIndex: this.index,
			localUser: this.user,
		}
	},
	updated() {
		this.localIndex = this.index;
		this.localUser = this.user;
	}
}
</script>

<style scoped>
.table button {
  margin-right: 30px;
}
.table button:last-of-type {
  margin-right: 0;
}
.table td:not(:last-of-type) {
  cursor: pointer;
}
</style>