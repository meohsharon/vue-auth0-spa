<template>
  <Menu as="div" class="relative inline-block text-left font-mono">
    <div>
      <MenuButton
        class="hidden sm:inline-flex w-full justify-center gap-x-1.5 rounded-md bg-white px-3 py-2 text-sm font-semibold text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 hover:bg-gray-50"
      >
        <img
          class="inline-block h-6 w-6 rounded-full"
          :src="user?.picture"
          alt="User Profile Picture"
        />
        {{ user?.name }}
        <ChevronDownIcon
          class="-mr-1 h-5 w-5 text-gray-600"
          aria-hidden="true"
        />
      </MenuButton>
      <MenuButton class="sm:hidden">
        <svg
          xmlns="http://www.w3.org/2000/svg"
          fill="none"
          viewBox="0 0 24 24"
          stroke-width="1.5"
          stroke="currentColor"
          class="w-[30px] text-red-500"
        >
          <path
            stroke-linecap="round"
            stroke-linejoin="round"
            d="M3.75 6.75h16.5M3.75 12h16.5m-16.5 5.25h16.5"
          />
        </svg>
      </MenuButton>
    </div>
    <transition
      enter-active-class="transition ease-out duration-100"
      enter-from-class="transform opacity-0 scale-95"
      enter-to-class="transform opacity-100 scale-100"
      leave-active-class="transition ease-in duration-75"
      leave-from-class="transform opacity-100 scale-100"
      leave-to-class="transform opacity-0 scale-95"
    >
      <MenuItems
        class="absolute right-0 z-10 mt-2 w-56 origin-top-right divide-y divide-gray-100 rounded-md bg-white shadow-lg ring-1 ring-black ring-opacity-5 focus:outline-none"
      >
        <div class="px-4 py-3 block sm:hidden" v-for="nav in topNavigation">
          <a :href="nav.target">{{ nav.text }}</a>
        </div>

        <div class="py-1">
          <div class="px-4 py-3">
            <p class="text-sm">Signed in as</p>
            <p class="truncate text-sm font-medium text-gray-900">
              {{ user?.email }}
            </p>
          </div>

          <MenuItem v-slot="{ active }">
            <button
              type="submit"
              @click="handleLogout"
              :class="[
                active ? 'bg-gray-100 text-gray-900' : 'text-gray-700',
                'block w-full px-4 py-2 text-left text-sm',
              ]"
            >
              Sign out
            </button>
          </MenuItem>
        </div>
      </MenuItems>
    </transition>
  </Menu>
</template>

<script setup lang="ts">
import { computed } from "vue";
import { useAuth0 } from "@auth0/auth0-vue";
import { Menu, MenuButton, MenuItem, MenuItems } from "@headlessui/vue";
import { ChevronDownIcon } from "@heroicons/vue/20/solid";
import { topNavigation } from "@helpers/lovs";

const auth0 = useAuth0();
const { logout } = auth0;

const user = computed(() => {
  return auth0.user.value;
});

const handleLogout = () => {
  // TODO: clear all the pinia stores here
  logout({
    logoutParams: {
      returnTo: window.location.origin,
    },
  });
};
</script>
