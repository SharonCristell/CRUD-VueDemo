<template>
  <div class="submit-form">
    <h3 style="color: blue">
   Añadir nuevo usuario
    </h3>
    <div v-if="!submitted">
      <div class="form-group">
        <label for="name">Nombre</label>
        <input
          type="text"
          class="form-control"
          id="name"
          required
          v-model="user.name"
          name="name"
        />
      </div>

      <div class="form-group">
        <label for="email">Email</label>
        <input
          class="form-control"
          id="email"
          required
          v-model="user.email"
          name="email"
        />
      </div>

      <button @click="saveUser" class="btn btn-success">
        Enviar datos
      </button>
    </div>

    <div v-else>
      <h4>Se añadió el usuario exitosamente!</h4>
      <button class="btn btn-success" @click="newUser">Add</button>
    </div>
  </div>
</template>

<script>
import UserDataService from "../services/UserDataService";

export default {
  name: "add-user",
  data() {
    return {
      user: {
        id: null,
        name: "",
        email: "",
      },
      submitted: false,
    };
  },
  methods: {
    saveuser() {
      var data = {
        name: this.user.name,
        email: this.user.email,
      };

      UserDataService.create(data)
        .then((response) => {
          this.user.id = response.data.id;
          console.log(response.data);
          this.submitted = true;
        })
        .catch((e) => {
          console.log(e);
        });
    },

    newuser() {
      this.submitted = false;
      this.user = {};
    },
  },
};
</script>

<style>
.submit-form {
  max-width: 300px;
  margin: auto;
  padding: 12px;
  border: 1px solid blue;
}
</style>
