<template>
  <header class="bg-at-light-green text-white">
    <nav
      class="
        container
        py-5
        px-4
        flex flex-column
        gap-4
        items-center
        sm:flex-row
      "
    >
      <div class="flex items-center gap-x-4">
        <img
          class="w-14"
          src="../assets/images/dumbbell-light.png"
          alt="dumbell"
        />
        <h1 class="text-lg">Activity tracker</h1>
      </div>
      <ul class="flex flex-1 justify-end gap-x-10">
        <router-link class="cursor-pointer" :to="{ name: 'Home' }"
          >Home</router-link
        >
        <router-link v-if="user" class="cursor-pointer" :to="{ name: 'Create' }"
          >Create</router-link
        >
        <router-link v-if="!user" class="cursor-pointer" :to="{ name: 'Login' }"
          >Login</router-link
        >
        <li v-if="user" @click="logout" class="cursor-pointer">Logout</li>
      </ul>
    </nav>
  </header>
</template>

<script>
import store from "../store/index";
import { computed } from "vue";
import { supabase } from "../supabase/init";
import { useRouter } from "vue-router";
export default {
  setup() {
    const user = computed(() => store.state.user);

    const router = useRouter();

    const logout = async () => {
      await supabase.auth.signOut();
      router.push({ name: "Home" });
    };

    return { logout, user };
  },
};
</script>
