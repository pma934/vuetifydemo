<template>
  <v-app id="app" :dark="dark">
    <!-- 抽屉 -->
    <v-navigation-drawer
      v-model="primaryDrawer.model"
      :permanent="primaryDrawer.type === 'permanent'"
      :temporary="primaryDrawer.type === 'temporary'"
      :clipped="primaryDrawer.clipped"
      :floating="primaryDrawer.floating"
      :mini-variant="primaryDrawer.mini"
      width="240"
      fixed
      app
      style="overflow: hidden;"
      dark
    >
      <v-img
        src="https://s2.ax1x.com/2019/06/16/VToxQH.jpg"
        gradient="to top right, rgba(100,115,201,.33), rgba(25,32,72,.7)"
        height="100%"
      >
        <v-list>
          <v-list-tile avatar>
            <v-list-tile-avatar color="white">
              <v-img src="https://avatars1.githubusercontent.com/u/44082279?s=460&v=4" height="34"/>
            </v-list-tile-avatar>
            <v-list-tile-title class="title">导航</v-list-tile-title>
          </v-list-tile>
        </v-list>

        <v-divider class="mx-3"/>

        <v-list class="pt-0">
          <v-list-tile
            v-for="item in items"
            :key="item.title"
            :to="item.to"
            class="py-2 px-3 my-v-list-tile"
          >
            <v-list-tile-action>
              <v-icon>{{ item.icon }}</v-icon>
            </v-list-tile-action>
            <v-list-tile-content>
              <v-list-tile-title class="font-weight-light">{{ item.title }}</v-list-tile-title>
            </v-list-tile-content>
          </v-list-tile>
        </v-list>

        <!-- <v-list class="pt-0 pl-4 pr-4" v-show="!primaryDrawer.mini">
        <canvas id="canvasTime" style="width:100%"></canvas>
        </v-list>-->
        <!-- <v-sheet class="pd-0 pt-0" v-show="!primaryDrawer.mini">
        <live2d
          style="position: fixed;right: -50px;bottom: 0px;z-index:-1"
          class="hidden-sm-and-down"
        ></live2d>
        </v-sheet>-->
      </v-img>
    </v-navigation-drawer>

    <!-- 顶部工具栏 -->
    <v-toolbar
      :clipped-left="primaryDrawer.clipped"
      :scroll-off-screen="!primaryDrawer.clipped"
      :absolute="!primaryDrawer.clipped"
      :flat="primaryDrawer.clipped"
      dense
      fixed
      app
      color="#ffffff66"
    >
      <v-toolbar-side-icon
        v-if="primaryDrawer.type !== 'permanent'"
        @click.stop="primaryDrawer.model = !primaryDrawer.model"
      ></v-toolbar-side-icon>
      <v-toolbar-title>围巾落地冻成狗</v-toolbar-title>
      <v-spacer></v-spacer>
      <v-toolbar-items class="hidden-sm-and-down">
        <v-btn icon @click.stop="dialog=!dialog">
          <v-icon>fa-cog</v-icon>
        </v-btn>
      </v-toolbar-items>
    </v-toolbar>

    <!-- <v-toolbar app flat prominent absolute >
      <v-img
        src="https://s2.ax1x.com/2019/06/16/VTTAfS.png"
        gradient="to top right, rgba(100,115,201,.33), rgba(25,32,72,.7)"
        height="100%"
      ></v-img>
    </v-toolbar>-->

    <!-- 内容 -->

    <v-content class="pt-0">
      <div></div>
      <div id="mask-box">
        <v-img src="https://s2.ax1x.com/2019/06/16/VTTAfS.png" height="120"></v-img>
      </div>
      <v-img src="https://s2.ax1x.com/2019/06/16/VTTAfS.png" height="120"></v-img>
      <v-container fluid>
        <!-- <v-layout align-center column> -->
        <keep-alive include="home,archive">
          <router-view style="border:1px solid"></router-view>
        </keep-alive>
        <!-- </v-layout> -->
      </v-container>
    </v-content>

    <!-- 页脚 -->
    <!-- <v-footer :inset="footer.inset" app>
      <span class="px-3">&copy; {{ new Date().getFullYear() }}</span>
    </v-footer>-->

    <!--对话框  -->
    <v-dialog v-model="dialog" max-width="500" scrollable lazy>
      <v-card>
        <v-toolbar flat dense color="blue">
          title
          <v-progress-linear indeterminate color="red"></v-progress-linear>
        </v-toolbar>
        <v-card-text>
          <v-layout row wrap>
            <v-flex xs12 md6>
              <span>Scheme</span>
              <v-switch v-model="dark" primary label="Dark"></v-switch>
            </v-flex>
            <v-flex xs12 md6>
              <span>Drawer</span>
              <v-radio-group v-model="primaryDrawer.type" column>
                <v-radio
                  v-for="drawer in drawers"
                  :key="drawer"
                  :label="drawer"
                  :value="drawer.toLowerCase()"
                  primary
                ></v-radio>
              </v-radio-group>
              <v-switch v-model="primaryDrawer.clipped" label="Clipped" primary></v-switch>
              <v-switch v-model="primaryDrawer.floating" label="Floating" primary></v-switch>
              <v-switch v-model="primaryDrawer.mini" label="Mini" primary></v-switch>
            </v-flex>
            <v-flex xs12 md6>
              <span>Footer</span>
              <v-switch v-model="footer.inset" label="Inset" primary></v-switch>
            </v-flex>
          </v-layout>
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn flat @click="dialog=false">Cancel</v-btn>
          <v-btn flat color="primary">Submit</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-app>
</template>

<script>
import canvasTime from "@/assets/javascript/canvasTime.js";
import Live2D from "./components/Live2D";
import GetBlog from "./plugins/getBlog.js";

export default {
  name: "app",
  data: () => ({
    items: [
      { title: "Home", icon: "fa-home", to: "/" },
      { title: "About", icon: "fa-address-card", to: "/about" },
      { title: "Grid", icon: "fa-th-large", to: "/grid" },
      { title: "Theme", icon: "fa-check-circle", to: "/theme" },
      { title: "归档", icon: "fa-check-circle", to: "/archive" }
    ],
    dialog: false,
    dark: false,
    drawers: ["Default (no property)", "Permanent", "Temporary"],
    primaryDrawer: {
      model: null,
      type: "default (no property)",
      clipped: false,
      floating: false,
      mini: false
    },
    footer: {
      inset: false
    },
    canvasTimeCallBack: null
  }),
  computed: {
    clockColor() {
      return this.$vuetify.theme.primary;
    }
  },
  watch: {
    clockColor: function() {
      this.canvasTimeCallBack = canvasTime(
        "canvasTime",
        this.clockColor,
        this.canvasTimeCallBack
      );
    }
  },
  components: {
    live2d: Live2D
  },
  beforeCreate() {
    GetBlog(this);
  },
  mounted() {
    // this.canvasTimeCallBack = canvasTime("canvasTime", this.clockColor);
  }
};
</script>

<style scoped>
.my-v-list-tile >>> .v-list__tile--active {
  background-color: var(--v-primary-base) !important;
  border-radius: 4px;
}
.my-v-list-tile >>> a {
  color: inherit !important;
}
#mask-box {
  height: 48px;
  margin-bottom: 72px;
  filter: blur(4px);
  overflow: hidden;
  position: absolute;
  width: 100%;
  z-index:1;
}
</style>

