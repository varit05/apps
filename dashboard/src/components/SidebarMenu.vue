<template>
  <div
    class="column happy-menu is-one-fifth is-one-third-mobile is-one-fifth-tablet"
    v-bind:class="{closed: isSidebarClosed }"
  >
    <router-link :to="{ name: 'settings' }">
      <figure class="image is-48x48 logo circle">
        <img
          src="../assets/koda_logo_843x843.png"
          alt="KodaDot logo"
        />
      </figure>
    </router-link
    <b-menu>
      <b-menu-list v-if="!isSidebarClosed" label="" icon-pack="fa">
        <b-menu-item
          class="menu-item"
          v-for="row in sidebar"
          v-bind:key="row.name"
          @click="currentRow = row"
          :icon="row.icon"
          :label="row.name"
          :tag="row.tag"
          :to="row.to"
        ></b-menu-item>
				<b-menu-item 
          class="menu-item"
          v-for="row in externalLinks"
          v-bind:key="row.name"
          @click="currentRow = row"
          :icon="row.icon"
          :icon-pack="row.pack"
					:label="row.name"
          :href="row.href"
          :target="row.target"
        ></b-menu-item>
      </b-menu-list>
      <b-menu-list v-if="isSidebarClosed">
        <b-menu-item 
          class="menu-item"
          v-for="row in sidebar"
          v-bind:key="row.name"
          @click="currentRow = row"
          :icon="row.icon"
          :tag="row.tag"
          :to="row.to"
        ></b-menu-item>
				<b-menu-item 
          class="menu-item"
          v-for="row in externalLinks"
          v-bind:key="row.name"
          @click="currentRow = row"
          :icon="row.icon"
          :icon-pack="row.pack"
          :href="row.href"
          :target="row.target"
        ></b-menu-item>
      </b-menu-list>
    </b-menu>
    <figure>
      <NetworkVisualCue />
    </figure>
    <b-switch v-if="!isSidebarClosed" v-model="showVerbose" 
      class="switchVerbose" type="is-danger">🕵️‍♂️</b-switch>
    <SettingInfo v-if="!isSidebarClosed && showVerbose" />

    <div class="toggleSidebar">
      <b-button class="closeSideBarBtn" v-if="!isSidebarClosed" 
        icon-left="angle-double-left" @click="toggleSidebar" rounded>
      </b-button>
      <b-button v-if="isSidebarClosed" icon-left="angle-double-right" 
        @click="toggleSidebar" rounded>
      </b-button>
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator';
import SettingInfo from '@/components/shared/SettingInfo.vue';
import NetworkVisualCue from '@/components/explorer/NetworkVisualCue.vue';

@Component({
  components: {
    SettingInfo,
    NetworkVisualCue,
  },
})
export default class SidebarMenu extends Vue {
  public sidebar: any = [
    {
      name: 'Explorer',
      icon: 'search',
      to: { name: 'explorer' },
      tag: 'router-link',
    },
    {
      name: 'Accounts',
      icon: 'users',
			to: { name: 'accounts'},
      tag: 'router-link',
    },
    {
      name: 'Address book',
      icon: 'address-book',
      to: { name: 'addressbook' },
      tag: 'router-link',
    },
    {
      name: 'Transfer',
      icon: 'paper-plane',
      to: { name: 'transfer' },
      tag: 'router-link',
    },
    {
      name: 'Democracy',
      icon: 'calendar-check',
      to: { name: 'democracy' },
      tag: 'router-link',
    },
    {
      name: 'Extrinsics',
      icon: 'sync',
      to: { name: 'extrinsics' },
      tag: 'router-link',
    },
    {
      name: 'Settings',
      icon: 'cogs',
      to: { name: 'settings' },
      tag: 'router-link',
    },
	];
	public externalLinks: any = [
		{
      name: 'Github',
			icon: 'code-branch',
      href: 'https://github.com/vue-polkadot/apps',
      target: '_blank'
    },
    {
      name: 'Wiki',
			icon: 'book',
      href: 'https://wiki.polkadot.network/',
      target: '_blank'
    },
    {
      name: '@KodaDot',
      icon: 'twitter',
      pack: 'fab',
      href: 'https://twitter.com/KodaDot',
      target: '_blank'
		},
	];
  public currentRow: any = this.sidebar[0];
  public isSidebarClosed = true;
  public showVerbose = false;
  public isActive = true;
  get hasBasicMode() {
    return this.$store.getters.getSettings.uiMode === 'light';
  }

  public toggleSidebar() {
    this.isSidebarClosed = !this.isSidebarClosed;
  }
}
</script>

<style>
.menu-list a {
  color: #dbdbdb;
}
.menu-list a.is-active {
  background-color: #40b883e0;
}
.happy-menu {
  background-color: #000000bd;
  min-height: 100%;
}

.menu-button {
  color: white;
}

.happy-menu.closed {
  width: 4em !important;
}

.image.is-48x48.logo {
  margin-bottom: 1rem;
}

.menu-item > a > span + span {
  vertical-align: super;
  padding-left: 0.5rem;
}
.closeSideBarBtn {
  margin-top: 0.7rem;
}
.switchVerbose {
  margin-top: 0.7rem;
}
.toggleSidebar {
  bottom: 10px;
  left: 15px;
  position: absolute;
  width: 100%;
}

.circle { 
   background: white; 
   -moz-border-radius: 70px; 
   -webkit-border-radius: 70px; 
   border-radius: 70px;
}
</style>
