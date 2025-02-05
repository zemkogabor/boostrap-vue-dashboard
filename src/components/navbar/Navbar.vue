<template>
  <nav class="navbar navbar-dark sticky-top bg-dark flex-md-nowrap p-0 shadow">
    <div class="navbar-brand col-md-3 col-lg-2 d-none d-md-block px-3 me-auto">
      {{ brandName }}
    </div>
    <button
      type="button"
      class="navbar-toggler d-md-none collapsed me-auto"
      data-bs-toggle="collapse"
      data-bs-target="#sidebarMenu"
      aria-controls="sidebarMenu"
      aria-expanded="false"
    >
      <span class="navbar-toggler-icon" />
    </button>
    <div v-if="navbarDropdown !== null" class="dropdown">
      <a
          id="navbarDropdownMenuLink"
          class="nav-link text-light"
          href="#"
          role="button"
          data-bs-toggle="dropdown"
          aria-expanded="false"
      >
        <i :class="navbarDropdown.iconClass" />
        <span v-if="navbarDropdown.title !== null" class="ms-2" v-text="navbarDropdown.title" />
      </a>
      <ul class="dropdown-menu dropdown-menu-end" aria-labelledby="navbarDropdownMenuLink">
        <li v-for="(dropdownItem, index) in navbarDropdown.items" :key="index">
          <a class="dropdown-item" href="#" @click="dropdownItem.callable">
            {{ dropdownItem.label }}
          </a>
        </li>
      </ul>
    </div>
  </nav>

  <div class="container-fluid">
    <div class="row">
      <nav id="sidebarMenu" class="col-md-3 col-lg-2 d-md-block bg-light sidebar collapse collapse-horizontal">
        <div class="position-sticky sidebar-content">
          <ul class="nav flex-column">
            <template v-for="(item, index) in items" :key="index">
              <div v-if="item.heading !== undefined" class="sidebar-heading px-3 mt-3 text-muted">
                {{ item.heading }}
              </div>
              <li v-for="(subItem, subItemIndex) in item.subItems" :key="subItemIndex" class="nav-item">
                <router-link
                  :to="subItem.route"
                  class="nav-link"
                  :class="{
                    'text-primary': subItem.route.name === $route.name
                  }"
                >
                  <i v-if="subItem.iconClass !== null" :class="subItem.iconClass" />
                  {{ subItem.label }}
                </router-link>
              </li>
            </template>
          </ul>
        </div>
      </nav>
      <main role="main" class="col-md-9 col-lg-10 ms-sm-auto">
        <div class="py-3">
          <slot />
        </div>
      </main>
    </div>
  </div>
</template>

<script>
/**
 * @typedef {Object} SidebarItem
 * @property {string|null} heading - Sidebar item heading.
 * @property {SubItem[]} subItems - Sidebar item sub items.
 *
 * @typedef {Object} SubItem
 * @property {string} label - Sub item label.
 * @property {string} iconClass - Sub item icon class.
 * @property {Route} route

 * @typedef {Object} Route
 * @property {string} name - Route name.
 *
 * @typedef {Object} NavbarDropdown
 * @param {string|null} title - Title
 * @param {string} iconClass - Icon class
 * @param {NavbarDropdownItem[]} items - Dropdown items.
 *
 * @typedef {Object} NavbarDropdownItem
 * @param {string} label - Label
 * @param {function} callable - Callable funcion onclick
 */
export default {
  name: 'Navbar',
  props: {
    brandName: {
      type: String,
      required: true,
    },
    items: {
      /** @type {SidebarItem[]} */
      type: Array,
      required: true,
    },
    navbarDropdown: {
      /** @type {NavbarDropdown} */
      type: Object,
      default: null,
    },
  },
}
</script>

<style lang="scss" scoped>
.sidebar {
  position: fixed;
  top: 0;
  bottom: 0;
  left: 0;
  z-index: 100; /* Behind the navbar */
  padding-top: 3.6rem;
  box-shadow: inset -1px 0 0 rgba(0, 0, 0, .1);

  // only SM
  @media (min-width: 576px) and (max-width: 768px) {
    width: 12rem;
  }

  .sidebar-content {
    width: 11.25rem;
  }
}

.sidebar .nav-link {
  font-weight: 500;
  color: #333;
  font-size: 85%;
}

.sidebar-heading {
  font-size: .75rem;
  text-transform: uppercase;
}

.navbar {
  height: 3rem;
}

.navbar-brand {
  padding-top: .75rem;
  padding-bottom: .75rem;
  font-size: 1rem;
  background-color: rgba(0, 0, 0, .25);
  box-shadow: inset -1px 0 0 rgba(0, 0, 0, .25);
}

.navbar-toggler {
  border: none;
}

.navbar-toggler:focus {
  box-shadow: none;
}

</style>
