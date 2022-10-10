<template>
    <!-- <div class="sidebar" :style="{ maxWidth: sidebarWidth }"> -->
  <div class="sidebar" :class="sidebarWidth">
    <router-link to="/dashboard">
      <h1>
        <i class="fa-solid fa-bolt" />
        <span v-if="collapsed"> </span>
        <span v-else style="margin-left: 10px">PVAnalyzer</span>
      </h1></router-link
    >
      <SidebarLink v-if="hasInstallation" to="/dashboard" icon="fas fa-home-alt" :collapsed="collapsed">Home</SidebarLink>
      <SidebarLink v-if="hasInstallation" to="/pv-installation" icon="fa-solid fa-solar-panel" :collapsed="collapsed">Moja instalacja</SidebarLink>
      <SidebarLink to="/profile" icon="fas fa-user" :collapsed="collapsed">{{ userName }}</SidebarLink>
      <SidebarLink @click="logout" icon="fa-solid fa-arrow-right-from-bracket" :collapsed="collapsed">Wyloguj</SidebarLink>
    <span
      class="collapse-icon"
      :class="{ 'rotate-180': collapsed }"
      @click="toggleSidebar"
    >
      <i class="fa-solid fa-angle-double-left" />
    </span>
  </div>
</template>

<script>
import { ref, computed } from 'vue'
import { useRoute } from 'vue-router'
import { useStore } from 'vuex'
import SidebarLink from "./SidebarLink.vue";
import logo from "../../assets/logo.png";
export default {
  props: {},
  components: { SidebarLink },
  setup() {
    const route = useRoute();
    const store = useStore();
    const collapsed = ref(false);
    const toggleSidebar = () => {
      (collapsed.value = !collapsed.value)
      store.commit('changeSidebarCollapse');
    }
    const sidebarWidth = computed(()=>{
      return collapsed.value ? 'sidebar-width-collapsed' : 'sidebar-width'
    })

    const userName = computed(()=> {
      return store.getters.userName;
    });
    const hasInstallation = computed(()=> {
      return store.getters["pVInstallation/hasInstallation"];
    });

    function logout(){
      store.dispatch("logout");
      route.replace('/login');
    }
    return {  collapsed, 
              toggleSidebar, 
              logo, 
              sidebarWidth,
              userName, 
              hasInstallation, 
              logout};
  },
};
</script>

<style>
:root {
  --sidebar-bg-color: #0934a5;
  --sidebar-item-hover: #083bbd;
  --sidebar-item-active: #041c58;
}
</style>

<style scoped>
h1 {
  font-size: 24px;
  color: white;
  padding: 0 0.4em 0 0.4em;
}
a {
  text-decoration: none;
}

.sidebar {
  color: white;
  background-color: var(--sidebar-bg-color);
  float: left;
  position: fixed;
  z-index: 1;
  top: 0;
  left: 0;
  bottom: 0;
  padding: 0.5em;
  transition: max-width 0.3s ease;
  display: flex;
  flex-direction: column;
  max-width: 180px;
  width: 180px;
  overflow: hidden;
  white-space: nowrap;
}
.sidebar-width-collapsed{
  max-width: 38px;
}
.sidebar-width{
  max-width: 180px;
}

.collapse-icon {
  position: absolute;
  bottom: 0;
  padding: 0.75em;

  color: rgba(255, 255, 255, 0.7);

  transition: 0.2s linear;
}

.rotate-180 {
  transform: rotate(180deg);
  transition: 0.2s linear;
}
</style>
