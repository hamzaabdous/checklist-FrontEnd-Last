<template>
  <v-card v-if="logged" class="mx-auto overflow-hidden">
    <v-app-bar color="#fff" style="border-bottom: 2px solid #002f6c">
      <v-app-bar-nav-icon v-if="this.fonction != 'driver'" @click="drawer = true"></v-app-bar-nav-icon>
      <v-toolbar-title>
        <v-img
          contain
          lazy-src="./assets/TangerAlliance.png"
          max-height="100"
          max-width="150"
          src="./assets/TangerAlliance.png"
        ></v-img>
      </v-toolbar-title>
      <p><b>Checklist</b></p>
      <v-spacer></v-spacer>
      <v-btn icon @click="logout">
        <v-icon>mdi-logout-variant</v-icon>
      </v-btn>
    </v-app-bar>

    <v-navigation-drawer
      v-model="drawer"
      absolute
      temporary
      width="15em"
      style="background-color: #fff"
    >
      <v-list style="padding: 0px; align-items: center" nav dense>
        <v-list-item-group active-class="deep-purple--text text--accent-4">
          <v-img
            contain
            lazy-src="./assets/TangerAlliance.png"
            max-height="220"
            max-width="250"
            src="./assets/TangerAlliance.png"
          ></v-img>

          <div
            v-if="this.fonction == 'technique' || this.fonction == 'foreman'"
            class="foremanandTechnique"
          >
            <div class="itemdrawer">
              <router-link class="itemd" to="/technique">Technique</router-link>
            </div>
          </div>
          <div
            v-if="this.fonction == 'driver' "
            class="foremanandTechnique"
          >
            <div class="itemdrawer">
              <router-link class="itemd" to="/Damage">Damage</router-link>
            </div>
          </div>

          <div v-if="this.fonction == 'admin' || this.fonction == 'adminIT'" class="admin">
            <div class="itemdrawer">
              <router-link class="itemd" to="/userGestion"
                >Gestion Users</router-link
              >
            </div>

            <div class="itemdrawer">
              <router-link class="itemd" to="/profile_groupe"
                >Equipment Profile</router-link
              >
            </div>
            <div class="itemdrawer">
              <router-link class="itemd" to="/technique">Technique</router-link>
            </div>
          </div>
        </v-list-item-group>
      </v-list>
    </v-navigation-drawer>
    <v-app style="background-color: #fff">
      <router-view />
    </v-app>
  </v-card>
  <Login v-else />
</template>

<script>
import { mapActions, mapGetters } from "vuex";
import Login from "./View/Login.vue";
export default {
  components: {
    Login,
  },
  data() {
    return {
      drawer: false,
      logged: false,
      fonction: "",
      listDrawerChildRouter: [
        { id: 1, name: "User", ROUTE: "/userGestion" },
        { id: 2, name: "Damage", ROUTE: "/Damage" },
        { id: 3, name: "Equipment Profile ", ROUTE: "/profile_groupe" },
        { id: 4, name: "technique ", ROUTE: "/technique" },
        { id: 5, name: "Dashboard ", ROUTE: "/Dashboard" },
      ],
    };
  },
  mounted() {
    if (this.getUserActive == null) {
      this.logged = false;
    } else if (this.getUserActive != null) {
      this.fonction = this.getUserActive.fonction.name;
      this.logged = true;
    }
  },
  computed: {
    ...mapGetters(["getUsers", "getUserActive"]),
  },
  watch: {},
  methods: {
    initialize() {},
    logout() {
      localStorage.clear();
      this.$router.push({
        name: "Login",
      });
      window.location.reload();
    },
    ...mapActions([]),
  },
};
</script>
