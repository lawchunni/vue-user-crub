<template>
  <div>
    <nav class="navbar is-dark" role="navigation" aria-label="main navigation">
      <div class="wrapper">
        <div class="navbar-brand">
          <div class="navbar-item">
            <h1>Advanced PHP Project - User CRUD</h1>
          </div>
        </div>
      </div>
      
    </nav>
    <div class="container is-max-desktop">

      <Transition name="slide-fade">
        <div v-if="successMsg.length" class="notification is-success">
          <button @click="successMsg = ''" class="delete"></button>
          <span v-text="successMsg"></span>
        </div>
      </Transition>
      <Transition name="slide-fade">
        <div v-if="errorMsg.length" class="notification is-danger">
          <button @click="errorMsg = ''" class="delete"></button>
          <span v-text="errorMsg"></span>
        </div>
      </Transition>

      <button @click="toggleFormModal({type:'create', mode:'open', index:null})" class="button is-primary create">Create New User</button>

      <Transition>
        <!-- All Users List -->
        <UserList :users="users" @toggleDetailsDisplay="toggleDetailsModal" @toggleFormDisplay="toggleFormModal" @deleteUser="deleteUser" />
      </Transition>
      <Transition>
        <!-- User Details -->
        <UserDetailsModal v-if="showDetailsModal" :user="user" @toggleDisplay="toggleDetailsModal" />
      </Transition>
      <Transition>
        <!-- User Form -->
        <FormModal v-if="showFormModal" :type="modalType" :user="user" @toggleDisplay="toggleFormModal" @formSubmit="formSubmit" />
      </Transition>
    </div>
  </div>
</template>

<script>
import UserList from './components/UserList.vue'
import UserDetailsModal from './components/UserDetailsModal.vue'
import FormModal from './components/FormModal.vue'

export default {
  name: 'App',
  data() {
    return {
      apiUrl: 'http://localhost:8001',
      showFormModal: false,
      showDetailsModal: false,
      modalType: '',
      users: [],
      user: {
        id: '',
        name:'',
        email: '',
        phone: '',
      },
      errorMsg: '',
      successMsg: '',
    }
  },
  components: {
    UserList,
    UserDetailsModal,
    FormModal,
  },
  methods: {
    getAllUsers() {
      fetch(this.apiUrl)
      .then(resp => resp.json())
      .then(json => {
        this.users = json.results.reverse();
      })
      .catch(error => {
        console.log(error);
      })
    },
    toggleDetailsModal({ user, display }) {
      this.user = user;
      this.showDetailsModal = display;
      this.successMsg = '';
      this.errorMsg = '';
    },
    toggleFormModal({ type, mode, index }) {
      this.modalType = type;

      if(type == 'edit' && mode == 'open') {
        // Edit user Modal opened
        this.user = { ...this.users[index] };
      } else {
        // Create user Modal opened or
        // Create / Edit Modals closed
        this.user = {
          id: '',
          name:'',
          email: '',
          phone: '',
        }
      }
      if (mode == 'open') {
        this.successMsg = '';
      } else {
        this.errorMsg = '';
      }
      this.showFormModal = !this.showFormModal;
    },
    formSubmit(user) {
      if(!user.name.length || !user.email.length|| !user.phone.length) {
        this.errorMsg = 'All fields required';
        return;
      }

      this.errorMsg = '';
      this.showFormModal = !this.showFormModal;

      let method = '';

      if(user.id) {
        method = 'PUT';
      } else {
        method = 'POST';
      }
      this.modifyUser(method, user)
    },
    deleteUser(user) {
      if(confirm(`Confirm to delete user: ${user.name}?`)) {
        this.modifyUser('DELETE', user);
      }
    },
    modifyUser(method, user) {
      fetch(this.apiUrl, {
        method: method,
        body: JSON.stringify(user),
        header: {
          'Content-Type': 'application/json'
        }
      })
      .then(resp => resp.json())
      .then(json => {
        if(json.status == 200 || json.status == 201) {
          this.successMsg = json.message;
          this.getAllUsers();
        } else {
          this.errorMsg = json.message;
        }
      })
    }
  },
  created() {
    this.getAllUsers();
  }
}
</script>

<style>
.navbar {
  margin-bottom: 60px;
}
.navbar .wrapper {
  width: 100%;
  max-width: 960px;
  margin: 0 auto;
}
.navbar .navbar-brand, .navbar-tabs {
  min-height: 3.9rem;
}
.navbar h1 {
  font-size: 1.4em;
}
button.create {
  margin-bottom: 30px;
}
.notification {
  position: fixed!important;
  max-width: 960px;
  top: 0px;
  width: 100%;
  z-index: 9999;
}
.modal-background {
  cursor: pointer;
}
/* Transition Effect*/
.v-enter-active,
.v-leave-active {
  transition: opacity 0.5s ease;
}
.v-enter-from,
.v-leave-to {
  opacity: 0;
}
.slide-fade-enter-active {
  transition: all 0.3s ease-out;
}
.slide-fade-leave-active {
  transition: all 0.3s cubic-bezier(1, 0.5, 0.8, 1);
}
.slide-fade-enter-from,
.slide-fade-leave-to {
  transform: translateX(20px);
  opacity: 0;
}
</style>
