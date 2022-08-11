<template>
	<div class="is-primary">
		<table class="table">
			<thead>
				<tr>
					<th>Id</th>
					<th>Name</th>
					<th>Email</th>
					<th>Phone</th>
					<th>Actions</th>
				</tr>
			</thead>

			<tbody>
				<UserListDetails 
					v-for="(user, index) in users" 
					:key="user.id" 
					:index="index" 
					:user="user"
					@toggleDetailsDisplay="$emit('toggleDetailsDisplay', $event)"
					@toggleFormDisplay="$emit('toggleFormDisplay', $event)"
					@deleteUser="$emit('deleteUser', $event)"
				/>
			</tbody>

		</table>
	</div>
</template>

<script>
import UserListDetails from './UserListDetails.vue';

export default {
	name: 'UserList',
	props: {
		users: Array,
		edit: Boolean,
	},
	components: {
		UserListDetails
	}
}
</script>

<style scoped>
.table {
  width: 100%;
}
</style>