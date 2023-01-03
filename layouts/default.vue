<template>
  <v-app dark>
    <v-navigation-drawer
      v-if="$auth.loggedIn ? $auth.user.account_type == 'Admin' : false"
      v-model="drawer"
      :mini-variant="miniVariant"
      :clipped="clipped"
      app
    >
      <v-list>
        <v-list-item
          v-for="(item, i) in items"
          :key="i"
          :to="item.to"
          router
          exact
        >
          <v-list-item-action>
            <v-icon>{{ item.icon }}</v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title v-text="item.title" />
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>
    <v-app-bar
      v-if="$auth.loggedIn ? $auth.user.account_type == 'Admin' : false"
      :clipped-left="clipped"
      fixed
      app
    >
      <v-app-bar-nav-icon @click.stop="drawer = !drawer" />
      <div align="start">
        <v-img
          class="pointer"
          src="/images/logo.jpg"
          height="50"
          width="100"
          contain
          @click="goIndex"
        />
      </div>
      <v-spacer />
      <div class="px-10 pointer">
        <v-btn dark depressed color="secondary" @click="$auth.logout()">
          Logout
        </v-btn>
      </div>
    </v-app-bar>

    <!-- is not logged in yet -->
    <v-app-bar color="white" v-else :clipped-left="clipped" fixed app>
      <div align="start">
        <v-img
          class="pointer"
          src="/images/logo.jpg"
          height="50"
          width="100"
          contain
          @click="goIndex"
        />
      </div>
      <v-spacer />
      <div
        :class="
          $route.name == 'contact'
            ? 'px-10 pointer secondary--text'
            : 'px-10 pointer'
        "
        @click="pushRoute('contact')"
      >
        Contact Us
      </div>
      <div
        :class="
          $route.name == 'client-profiles'
            ? 'px-10 pointer secondary--text'
            : 'px-10 pointer'
        "
      >
        Travel Requirements
      </div>
      <div
        :class="
          $route.name == 'login'
            ? 'px-10 pointer secondary--text'
            : 'px-10 pointer'
        "
        @click="pushRoute('login')"
        v-if="!$auth.loggedIn"
      >
        Login
      </div>
      <div
        :class="
          $route.name == 'client-profile'
            ? 'px-10 pointer secondary--text'
            : 'px-10 pointer'
        "
        @click="pushRoute('client/profile')"
        v-else
      >
        My Profile
      </div>
      <div class="px-10 pointer" v-if="!$auth.loggedIn">
        <v-btn dark depressed color="secondary" @click="pushRoute('register')">
          Sign up
        </v-btn>
      </div>
      <div class="px-10 pointer" v-else>
        <v-btn dark depressed color="secondary" @click="$auth.logout()">
          Logout
        </v-btn>
      </div>
    </v-app-bar>
    <v-main>
      <v-container fluid class="pa-0">
        <Nuxt />
      </v-container>
    </v-main>
    <v-navigation-drawer v-model="rightDrawer" :right="right" temporary fixed>
      <v-list>
        <v-list-item @click.native="right = !right">
          <v-list-item-action>
            <v-icon light> mdi-repeat </v-icon>
          </v-list-item-action>
          <v-list-item-title>Switch drawer (click me)</v-list-item-title>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>
    <v-footer padless>
      <v-card flat tile>
        <v-card-text>
          <v-btn
            v-for="icon in iconFooter"
            :key="icon"
            class="mx-4 secondary--text"
            icon
          >
            <v-icon size="24px" @click="goToFb">
              {{ icon }}
            </v-icon>
          </v-btn>
        </v-card-text>

        <v-card-text class="black--text pt-0">
          Evaristo & Sons Sea Transport Corporation is a domestic shipping
          company operating cargo and passenger ships
        </v-card-text>

        <v-divider></v-divider>

        <v-card-text class="secondary--text">
          <a href="https://www.freepik.com/vectors/claim"
            >Claim vector created by pch.vector - www.freepik.com</a
          >
          {{ new Date().getFullYear() }} — <strong>R2M</strong>
        </v-card-text>
      </v-card>
    </v-footer>
  </v-app>
</template>

<script>
export default {
  methods: {
    goToFb() {
      location = "https://www.facebook.com/evaristoandsonsseatransportcorp";
    },
    pushRoute(link) {
      window.location.href = `/${link}`;
    },
    goIndex() {
      window.location.href = "/";
    },
  },
  name: "DefaultLayout",
  data() {
    return {
      clipped: false,
      drawer: false,
      fixed: false,
      iconFooter: [
        "mdi-facebook",
        // "mdi-twitter",
        // "mdi-linkedin",
        // "mdi-instagram",
      ],
      items: [
        {
          icon: "mdi-chart-bubble",
          title: "Book",
          to: "/admin/book",
        },
        {
          icon: "mdi-chart-bubble",
          title: "Departure",
          to: "/admin/departure",
        },
        {
          icon: "mdi-chart-bubble",
          title: "Return",
          to: "/admin/return",
        },
        {
          icon: "mdi-chart-bubble",
          title: "Report",
          to: "/admin/report",
        },
      ],
      miniVariant: false,
      right: true,
      rightDrawer: false,
      title: "Vuetify.js",
    };
  },
};
</script>
