<template>
  <nav class="navbar navbar-dark sticky-top bg-dark flex-md-nowrap p-0 shadow">
    <div class="navbar-brand col-md-3 col-lg-2 me-0 px-3">
      {{ brandName }}
    </div>

    <button
      type="button"
      class="navbar-toggler position-absolute d-md-none collapsed"
      data-bs-toggle="collapse"
      data-bs-target="#sidebarMenu"
      aria-controls="sidebarMenu"
      aria-expanded="false"
    >
      <span class="navbar-toggler-icon" />
    </button>
  </nav>

  <div class="container-fluid">
    <div class="row">
      <nav id="sidebarMenu" class="col-md-3 col-lg-2 d-md-block bg-light sidebar collapse">
        <div class="position-sticky">
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
 * @typedef {{
 *   ?heading: String,
 *   subItems: {
 *     label: String,
 *     ?iconClass: String,
 *     route: {
 *       name: String
 *     },
 *   }[],
 * }} Item
 */
export default {
  name: 'Navbar',
  props: {
    brandName: {
      type: String,
      required: true,
    },
    items: {
      /** @type {{ new(): Item[] }} */
      type: Array,
      required: true,
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

.navbar-brand {
  padding-top: .75rem;
  padding-bottom: .75rem;
  font-size: 1rem;
  background-color: rgba(0, 0, 0, .25);
  box-shadow: inset -1px 0 0 rgba(0, 0, 0, .25);
}

.navbar .navbar-toggler {
  top: .25rem;
  right: 1rem;
}
</style>
