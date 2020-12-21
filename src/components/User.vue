<template>
  <div v-if="currentUser" class="edit-form">
    <h4>Usuario</h4>
    <form>
      <div class="form-group">
        <label for="name">Nombre</label>
        <input type="text" class="form-control" id="name"
          v-model="currentUser.name"
        />
      </div>
      <div class="form-group">
        <label for="email">Email</label>
        <input type="email" class="form-control" id="email"
          v-model="currentUser.email"
        />
      </div>
    </form>


    <button class="btn btn-outline-danger mr-2"
      @click="deleteUser"
    >
      Borrar
    </button>

    <button type="submit" class="btn btn-outline-primary"
      @click="updateUser"
    >
      Actualizar
    </button>
    <p>{{ message }}</p>
  </div>

  <div v-else>
    <br />
    <p>Espere un momento por favor mientras se cargan los datos del usuario...</p>
  </div>
</template>

<script>
import UserDataService from "../services/UserDataService";

export default {
  name: "user",
  data() {
    return {
      currentUser: null,
      message: ''
    };
  },
  methods: {
    getUser(id) {
      UserDataService.get(id)
        .then(response => {
          this.currentUser = response.data;
          console.log(response.data);
        })
        .catch(e => {
          console.log(e);
        });
    },


    updateUser() {
      UserDataService.update(this.currentUser.id, this.currentUser)
        .then(response => {
          console.log(response.data);
          this.message = 'El usuario fue actualizado exitosamente!';
        })
        .catch(e => {
          console.log(e);
        });
    },

    deleteUser() {
      UserDataService.delete(this.currentUser.id)
        .then(response => {
          console.log(response.data);
          this.$router.push({ name: "users" });
        })
        .catch(e => {
          console.log(e);
        });
    }
  },
  mounted() {
    this.message = '';
    this.getUser(this.$route.params.id);
  }
};
</script>

<style>
.edit-form {
  max-width: 300px;
  margin: auto;
}
</style>
