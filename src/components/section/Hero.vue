<template>
  <section class="hero is-info is-medium is-bold">

    <div class="hero-head">
      <nav class="navbar">
        <div class="container">
          <div class="navbar-brand">
            <a href="/" class="navbar-item">
              <img src="../../assets/logo.png" height="100%" class="mr-2">
              <h1 class="title is-4">Bike Gears</h1>
            </a>

            <!-- Burger menu only shown on touch devices -->
            <a role="button" :class="{'navbar-burger': true, 'is-active': showMobileMenu}" aria-label="menu"
               aria-expanded="false" @click="showMobileMenu = ! showMobileMenu">
              <span aria-hidden="true"></span>
              <span aria-hidden="true"></span>
              <span aria-hidden="true"></span>
            </a>
          </div>

          <div :class="{'navbar-menu': true, 'is-active': showMobileMenu}">

            <div class="navbar-end">

              <router-link to="/" :class="{'navbar-item': true, 'is-active': isActivePath('/')}">Calculator
              </router-link>
              <router-link to="/learn" :class="{'navbar-item': true, 'is-active': isActivePath('/learn')}">Learn
              </router-link>

              <span v-if="showDonateButton" class="navbar-item">
                <DonateButton></DonateButton>
              </span>

            </div>
          </div>
        </div>
      </nav>
    </div>

    <div class="hero-body">
      <div class="container has-text-centered">
        <div class="select is-large is-rounded is-info">
          <select v-model="currentType" @change="calculationChanged">
            <option :disabled="option.disabled" :key="option.value" v-for="option in calculationOptions"
                    :value="option.value">{{ option.text }}
            </option>
          </select>
        </div>
      </div>
    </div>

    <div class="hero-foot has-text-right">
      <div id="dark-mode-item" class="mr-2 mb-2">
        <DarkModeSwitch></DarkModeSwitch>
      </div>
    </div>

  </section>
</template>

<script>
import {CALCULATION_TYPES} from "@/constants";
import {mapMutations} from 'vuex';
import DarkModeSwitch from "@/components/partials/DarkModeSwitch";
import DonateButton from "@/components/partials/DonateButton";

export default {
  name: "Hero",
  components: {DonateButton, DarkModeSwitch},
  computed: {
    calculationOptions() {
      return CALCULATION_TYPES.map(type => {
        return {
          value: type.id,
          text: type.name,
          disabled: !type.enabled
        }
      });
    }
  },
  data() {
    return {
      currentType: CALCULATION_TYPES[0].id,
      showMobileMenu: false,
      showDonateButton: false
    }
  },
  mounted() {
    this.calculationChanged();
  },
  methods: {
    ...mapMutations(['setCalculationType']),
    calculationChanged() {
      this.setCalculationType(CALCULATION_TYPES.filter(type => type.id === this.currentType)[0]);
    },
    isActivePath(path) {
      return this.$route.path === path;
    },
  }
}
</script>

<style scoped lang="scss">
@import '~bulma/sass/utilities/all';

.title {
  text-transform: uppercase;
}

[data-theme="dark"] {
  section.hero.is-info {
    background-image: linear-gradient(to bottom, rgba(0, 0, 0, .2), rgba(0, 0, 0, 0.6)),
    url('../../assets/hero.jpg');
  }
}

section.hero.is-info {

  background-image: linear-gradient(to bottom, rgba(0, 0, 0, .3), rgba(0, 0, 0, 0.75)),
  url('../../assets/hero.jpg');
  background-size: cover;
  background-position-y: bottom;

  .navbar-brand {
    .navbar-item {
      background-color: transparent;

      img {
        position: absolute;
        height: 50px;
        max-height: none;
      }

      h1 {
        margin-left: 4.5rem;
      }
    }

    .navbar-burger {
      color: white;
    }
  }

  .navbar-menu {
    a.navbar-item.is-active {
      background-color: rgba(20, 20, 20, .7);
      border-bottom-left-radius: 10px;
      border-bottom-right-radius: 10px;
      transition: background-color .3s ease;
    }

    a.navbar-item:not(.is-active) {
      background-color: transparent;
    }

    &.is-active {
      position: absolute;
      width: 100%;
      text-align: left;
      background-color: rgba(20, 20, 20, .93);
      background-image: unset;
    }
  }
}

#dark-mode-item {
  .switch[type=checkbox] + label {
    font-size: .8rem;
  }
}
</style>
