<template>
  <div>
    <div class="list row">
      <div class="col-md-8">
        <div class="input-group mb-3">
          <input
            type="text"
            class="form-control"
            placeholder="Buscar por nombre"
            v-model="title"
          />
          <div class="input-group-append">
            <button
              class="btn btn-outline-secondary"
              type="button"
              @click="searchName"
            >
              Buscar
            </button>
          </div>
        </div>
      </div>
      <div class="col-md-6">
        <h4>Lista de Usuarios</h4>
        <ul class="list-group">
          <li
            class="list-group-item"
            :class="{ active: index == currentIndex }"
            v-for="(user, index) in users"
            :key="index"
            @click="setActiveUser(user, index)"
          >
            {{ user.name }}
          </li>
        </ul>

        <button class="m-3 btn btn-sm btn-danger" @click="removeAllUsers">
          Borrar Todos
        </button>
      </div>
      <div class="col-md-6">
        <div v-if="currentUser">
          <h4>Usuario</h4>
          <div>
            <label><strong>Identificador:</strong></label> {{ currentUser.id }}
          </div>
          <div>
            <label><strong>Nombre:</strong></label> {{ currentUser.name }}
          </div>
          <div>
            <label><strong>Email:</strong></label> {{ currentUser.email }}
          </div>

          <router-link
            :to="'/users/' + currentUser.id"
            class="btn btn-outline-success"
            >Editar</router-link
          >
        </div>
        <div v-else>
          <br />
          <p>Para ver más detalles del usuario, por favor, seleccionelo...</p>
        </div>
      </div>
    </div>
      <div class="list row">
   
     <h3 style="color:blue;">
          Code Challenge desarrollado por
          <span  style="color:orange;" class="typed-text">{{ typeValue }}</span>
          <span  style="color:orange;" class="cursor" :class="{ typing: typeStatus }">&nbsp;</span>
        </h3>
       
  
   </div>
  </div>
</template>

<script>
import UserDataService from "../services/UserDataService";
import { setTimeout } from "timers";

export default {
  name: "users-list",
  data() {
    return {
      users: [],
      currentUser: null,
      currentIndex: -1,
      name: "",
      typeValue: "",
      typeStatus: false,
      typeArray: ["Sharon Cristell", "Quispe Carhuapoma"],
      typingSpeed: 200,
      erasingSpeed: 100,
      newTextDelay: 2000,
      typeArrayIndex: 0,
      charIndex: 0,
      slide: 0,
      sliding: null,
    };
  },
  methods: {
    retrieveUsers() {
      UserDataService.getAll()
        .then((response) => {
          this.users = response.data;
          console.log(response.data);
        })
        .catch((e) => {
          console.log(e);
        });
    },

    refreshList() {
      this.retrieveUsers();
      this.currentUser = null;
      this.currentIndex = -1;
    },

    setActiveUser(user, index) {
      this.currentUser = user;
      this.currentIndex = index;
    },

    removeAllUsers() {
      UserDataService.deleteAll()
        .then((response) => {
          console.log(response.data);
          this.refreshList();
        })
        .catch((e) => {
          console.log(e);
        });
    },

    searchName() {
      UserDataService.findByName(this.name)
        .then((response) => {
          this.users = response.data;
          console.log(response.data);
        })
        .catch((e) => {
          console.log(e);
        });
    },

    typeText() {
      if (this.charIndex < this.typeArray[this.typeArrayIndex].length) {
        if (!this.typeStatus) this.typeStatus = true;
        this.typeValue += this.typeArray[this.typeArrayIndex].charAt(
          this.charIndex
        );
        this.charIndex += 1;
        setTimeout(this.typeText, this.typingSpeed);
      } else {
        this.typeStatus = false;
        setTimeout(this.eraseText, this.newTextDelay);
      }
    },
    eraseText() {
      if (this.charIndex > 0) {
        if (!this.typeStatus) this.typeStatus = true;
        this.typeValue = this.typeArray[this.typeArrayIndex].substring(
          0,
          this.charIndex - 1
        );
        this.charIndex -= 1;
        setTimeout(this.eraseText, this.erasingSpeed);
      } else {
        this.typeStatus = false;
        this.typeArrayIndex += 1;
        if (this.typeArrayIndex >= this.typeArray.length)
          this.typeArrayIndex = 0;
        setTimeout(this.typeText, this.typingSpeed + 1000);
      }
    },
  },

  created() {
    setTimeout(this.typeText, this.newTextDelay + 200);
  },
  mounted() {
    this.retrieveUsers();
  },
};
</script>

<style>
.list {
  text-align: left;
  max-width: 750px;
  margin: auto;
  padding: 12px;
 
}
</style>
