<script setup>
import { computed, ref } from 'vue'
import { useMainStore } from '@/stores/main'
import {
  mdiForwardburger,
  mdiBackburger,
  mdiClose,
  mdiDotsVertical,
  mdiMenu,
  mdiClockOutline,
  mdiCloud,
  mdiCrop,
  mdiAccount,
  mdiCogOutline,
  mdiEmail,
  mdiLogout,
  mdiGithub,
  mdiThemeLightDark
} from '@mdi/js'
import NavBarItem from '@/components/NavBarItem.vue'
import NavBarItemLabel from '@/components/NavBarItemLabel.vue'
import NavBarMenu from '@/components/NavBarMenu.vue'
import Divider from '@/components/Divider.vue'
import UserAvatar from '@/components/UserAvatar.vue'
import Icon from '@/components/Icon.vue'
import NavBarSearch from '@/components/NavBarSearch.vue'

const mainStore = useMainStore()

const lightBorderStyle = computed(() => mainStore.lightBorderStyle)

const toggleLightDark = () => {
  mainStore.setDarkMode()
}

const isNavBarVisible = computed(() => !mainStore.isFullScreen)

const isAsideMobileExpanded = computed(() => mainStore.isAsideMobileExpanded)

const userName = computed(() => mainStore.userName)

const menuToggleMobileIcon = computed(() => isAsideMobileExpanded.value ? mdiBackburger : mdiForwardburger)

const menuToggleMobile = () => mainStore.asideMobileToggle()

const isMenuNavBarActive = ref(false)

const menuNavBarToggleIcon = computed(() => isMenuNavBarActive.value ? mdiClose : mdiDotsVertical)

const menuNavBarToggle = () => {
  isMenuNavBarActive.value = !isMenuNavBarActive.value
}

const menuOpenLg = () => {
  mainStore.asideLgToggle(true)
}

const logout = () => {
  //
}
</script>

<template>
  <nav
    v-show="isNavBarVisible"
    class="top-0 left-0 right-0 fixed flex bg-white h-14 border-b z-30 w-screen
    transition-position xl:pl-60 lg:w-auto lg:items-stretch dark:bg-gray-900 dark:border-gray-800"
    :class="[lightBorderStyle, {'ml-60 lg:ml-0':isAsideMobileExpanded}]"
  >
    <div class="flex-1 items-stretch flex h-14">
      <NavBarItem
        type="flex lg:hidden"
        @click.prevent="menuToggleMobile"
      >
        <Icon
          :path="menuToggleMobileIcon"
          size="24"
        />
      </NavBarItem>
      <NavBarItem
        type="hidden lg:flex xl:hidden"
        @click.prevent="menuOpenLg"
      >
        <Icon
          :path="mdiMenu"
          size="24"
        />
      </NavBarItem>
      <NavBarItem>
        <NavBarSearch />
      </NavBarItem>
    </div>
    <div class="flex-none items-stretch flex h-14 lg:hidden">
      <NavBarItem @click.prevent="menuNavBarToggle">
        <Icon
          :path="menuNavBarToggleIcon"
          size="24"
        />
      </NavBarItem>
    </div>
    <div
      class="absolute w-screen top-14 left-0 bg-white shadow
        lg:w-auto lg:items-stretch lg:flex lg:grow lg:static lg:border-b-0 lg:overflow-visible lg:shadow-none dark:bg-gray-900"
      :class="[isMenuNavBarActive ? 'block' : 'hidden']"
    >
      <div
        class="max-h-screen-menu overflow-y-auto lg:overflow-visible lg:flex lg:items-stretch lg:justify-end lg:ml-auto"
      >
        <NavBarMenu has-divider>
          <NavBarItemLabel
            :icon="mdiMenu"
            label="Sample menu"
          />

          <template #dropdown>
            <NavBarItem>
              <NavBarItemLabel
                :icon="mdiClockOutline"
                label="Item One"
              />
            </NavBarItem>
            <NavBarItem>
              <NavBarItemLabel
                :icon="mdiCloud"
                label="Item Two"
              />
            </NavBarItem>
            <Divider nav-bar />
            <NavBarItem>
              <NavBarItemLabel
                :icon="mdiCrop"
                label="Item Last"
              />
            </NavBarItem>
          </template>
        </NavBarMenu>
        <NavBarMenu has-divider>
          <NavBarItemLabel :label="userName">
            <UserAvatar class="w-6 h-6 mr-3 inline-flex" />
          </NavBarItemLabel>

          <template #dropdown>
            <NavBarItem to="/profile">
              <NavBarItemLabel
                :icon="mdiAccount"
                label="My Profile"
              />
            </NavBarItem>
            <NavBarItem>
              <NavBarItemLabel
                :icon="mdiCogOutline"
                label="Settings"
              />
            </NavBarItem>
            <NavBarItem>
              <NavBarItemLabel
                :icon="mdiEmail"
                label="Messages"
              />
            </NavBarItem>
            <Divider nav-bar />
            <NavBarItem>
              <NavBarItemLabel
                :icon="mdiLogout"
                label="Log Out"
              />
            </NavBarItem>
          </template>
        </NavBarMenu>
        <NavBarItem
          has-divider
          is-desktop-icon-only
          @click.prevent="toggleLightDark"
        >
          <NavBarItemLabel
            :icon="mdiThemeLightDark"
            label="Light/Dark"
            is-desktop-icon-only
          />
        </NavBarItem>
        <NavBarItem
          href="https://github.com/justboil/admin-one-vue-tailwind"
          has-divider
          is-desktop-icon-only
        >
          <NavBarItemLabel
            :icon="mdiGithub"
            label="GitHub"
            is-desktop-icon-only
          />
        </NavBarItem>
        <NavBarItem
          is-desktop-icon-only
          @click="logout"
        >
          <NavBarItemLabel
            :icon="mdiLogout"
            label="Log out"
            is-desktop-icon-only
          />
        </NavBarItem>
      </div>
    </div>
  </nav>
</template>
