<script setup>
import { ref, onMounted, onUnmounted } from 'vue';
import { useRoute } from 'vue-router';

const isScrolled = ref(false);
const route = useRoute();

const menus = [  { name: 'Home', path: '/' },  { name: 'About', path: '/about' },  {    name: 'Browse',    path: '/browse',    children: [      { name: 'Event List', path: '/browse/events' },      { name: 'Category', path: '/browse/category' },      { name: 'Your Events', path: '/browse/your-events' }    ]
  },
  { name: 'Contact', path: '/contact' },
  { name: 'Organizer Dashboard', path: '/dashboard' }
];

const handleScroll = () => {
  isScrolled.value = window.scrollY > 10;
};

onMounted(() => {
  window.addEventListener('scroll', handleScroll);
});

onUnmounted(() => {
  window.removeEventListener('scroll', handleScroll);
});
</script>

<template>
  <nav class="navbar" :class="{ scrolled: isScrolled }">
    <div class="navbar-container">
      <router-link to="/" class="logo">
        <svg width="28" height="32" viewBox="0 0 24 28" fill="none" xmlns="http://www.w3.org/2000/svg" class="logo-icon">
          <path d="M12 0L22.3923 6V18L12 24L1.6077 18V6L12 0Z" fill="white" />
          <path d="M15 9.5 C15 9.5 14 8 12 8 C9.5 8 8 10 8 12.5 C8 15 9.5 17 12 17 C14 17 15 16 15.5 14.5 V 12.5 H 12.5" stroke="#1A1643" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round" />
        </svg>
        <span class="logo-text">Gatherly</span>
      </router-link>
      <ul class="nav-menu">
        <li class="nav-item" v-for="menu in menus" :key="menu.name">
          <router-link
            :to="menu.path"
            class="nav-link"
            :class="{
              active: route.path === menu.path || (menu.path !== '/' && route.path.startsWith(menu.path))
            }"
          >
            {{ menu.name }}
            <svg v-if="menu.children" class="dropdown-indicator" width="12" height="12" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
              <polyline points="6 9 12 15 18 9"></polyline>
            </svg>
          </router-link>
          <ul v-if="menu.children" class="dropdown-menu">
            <li v-for="child in menu.children" :key="child.name" class="dropdown-item">
              <router-link :to="child.path" class="dropdown-link">
                {{ child.name }}
              </router-link>
            </li>
          </ul>
        </li>
      </ul>
      <div class="nav-right">
        <div class="lang-selector">
          <span class="lang-text">EN</span>
        </div>
      </div>
    </div>
  </nav>
</template>

<style scoped>
.navbar {
  width: 100%;
  background: var(--nav-bg, #1c1948);
  border-bottom: 1px solid var(--nav-border, #333);
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.15);
  position: sticky;
  top: 0;
  z-index: 999;
  transition: all 0.3s ease;
}
.navbar-container {
  display: flex;
  align-items: center;
  justify-content: space-between;
  width: 100%;
  max-width: 1440px;
  margin: 0 auto;
  padding: 0.85rem 2rem;
}
.logo {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  text-decoration: none;
  color: #fff;
}
.logo-text {
  font-size: 1.15rem;
  font-weight: 700;
}
.nav-menu {
  display: flex;
  align-items: center;
  gap: 2.5rem;
  list-style: none;
  margin: 0;
  padding: 0;
  flex: 1;
  justify-content: center;
}
.nav-item {
  position: relative;
  display: flex;
  align-items: center;
  height: 100%;
  padding: 1rem 0;
}
.nav-link {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  text-decoration: none;
  color: #fff;
  font-size: 0.95rem;
  font-weight: 500;
  padding: 0.5rem 1.25rem;
  border-radius: 10px;
  transition: all 0.3s ease;
}
.nav-link.active {
  background-color: var(--primary, #6644ff);
  font-weight: 600;
}
.dropdown-menu {
  display: none;
  position: absolute;
  top: 100%;
  left: 0;
  background-color: #d8d8d8;
  min-width: 200px;
  list-style: none;
  padding: 0;
  margin: 0;
  box-shadow: 0 8px 16px rgba(0, 0, 0, 0.2);
  border-radius: 4px;
}
.nav-item:hover .dropdown-menu {
  display: block;
}
.dropdown-link {
  display: flex;
  justify-content: space-between;
  padding: 12px 20px;
  text-decoration: none;
  color: #333;
  font-size: 0.95rem;
  border-bottom: 1px solid rgba(0, 0, 0, 0.05);
}
.dropdown-link:hover {
  background-color: #c4c4c4;
  color: #000;
}
.nav-right {
  display: flex;
  align-items: center;
  gap: 1.5rem;
}
.lang-selector {
  color: #fff;
  font-size: 0.9rem;
  font-weight: 600;
  cursor: pointer;
}
</style>