<template>
  <div>
    <h3>Server Canvas</h3>
    <div class="d-flex flex-wrap flex-row">
      <div v-for="(server, index) in servers" :key="index" class="server-container rounded m-3">
        <transition name="fade">
          <div class="app-container text-center" v-if="server.apps.length > 0">
            <div v-for="(app, index) in server.apps" :key="index" class="app p-3" :class="app.appName">
              <div class="icon-text">{{ app.iconText }}</div>
              <div class="app-name">{{ app.appName }}</div>
              <div class="time-stamp mt-3">Added {{ app.duration }}</div>
            </div>
          </div>
        </transition>
      </div>
    </div>
  </div>
</template>
<script>
import { EventBus } from "./../event-bus.js";
import * as moment from 'moment';

export default {
  data() {
    return {
      servers: [{
          apps: []
        },
        {
          apps: []
        },
        {
          apps: []
        },
        {
          apps: []
        }
      ]
    };
  },
  mounted() {
    EventBus.$on("add-app", appname => {
      this.addApp(appname);
    });
    EventBus.$on("remove-app", appname => {
      this.removeApp(appname);
    });
    EventBus.$on("add-server", () => {
      this.addServer();
    });
    EventBus.$on("destroy-server", () => {
      this.destroyServer();
    });
  },
  methods: {
    addServer() {
      let newServer = this.createServer();
      this.servers.push(newServer);
    },
    createServer() {
      return {
        apps: []
      };
    },
    destroyServer() {
      //remove last added server
      let server = this.servers.pop();

      if (server.apps.length !== 0) {
        server.apps.forEach(app => {
          this.reallocateApp(app);
        });
      }
    },
    reallocateApp(app) {
      let serverIndex = this.findFreeServer();
      if (!serverIndex) {
        console.log("No space!!!");
      } else {
        this.servers[serverIndex].apps.push(app);
      }
    },
    addApp(appname) {
      let serverIndex = this.findFreeServer();
      if (!serverIndex) {
        console.log("No space!!!");
      } else {
        let newApp = this.createApp(appname);
        this.servers[serverIndex].apps.push(newApp);
      }
    },
    createApp(appname) {
      let app = {
        appName: appname,
        iconText: appname === "Hadoop" ? "Hd" : appname.substr(0, 2),
        time: moment(),
        duration: moment().fromNow()
      };
      return app;
    },
    findFreeServer() {
      let freeServerIndex = undefined;

      for (let index in this.servers) {
        if (this.servers[index].apps.length === 0) {
          freeServerIndex = index;
          break;
        }
      }

      if (!freeServerIndex) {
        for (let index in this.servers) {
          if (this.servers[index].apps.length === 1) {
            freeServerIndex = index;
            break;
          }
        }
      }

      return freeServerIndex;
    },
    removeApp(appname) {
      let lastAddedAppIndex = undefined;

      for (let index in this.servers) {
        if (this.servers[index].apps.length === 2 && this.servers[index].apps[1].appName === appname) {
          lastAddedAppIndex = index;
        }
      }

      if (lastAddedAppIndex) {
        this.servers[lastAddedAppIndex].apps.pop();
        return;

      }

      for (let index in this.servers) {
        if (this.servers[index].apps.length > 0 && this.servers[index].apps[0].appName === appname) {
          lastAddedAppIndex = index;
        }
      }

      if (lastAddedAppIndex) {
        this.servers[lastAddedAppIndex].apps.shift();
        return;
      }
    }
  }
};
</script>
<style lang="less" scoped>
.server-container {
  width: 240px;
  height: 120px;
  background: #2f2f2f;
}

.app-container {
  height: 100%;

  .app {
    display: inline-block;
    width: 50%;
    font-weight: bold;

    &.Hadoop {
      background-image: linear-gradient(to left top, #9c00a7, #92039e, #880595, #7e068c, #740783);
    }

    &.Rails {
      background-image: linear-gradient(to left top, #0477a7, #007ba1, #007f98, #00818d, #078381);
    }

    &.Chronos {
      background-image: linear-gradient(to left top, #0043a7, #044bba, #0a53cc, #125ce0, #1964f3);
    }

    &.Storm {
      background-image: linear-gradient(to left top, #93a70f, #a5b31c, #b7c028, #c9cc33, #dcd93d);
    }

    &.Spark {
      background-image: linear-gradient(to right bottom, #00a745, #00b048, #00ba4b, #00c34d, #01cd50);
    }

    .icon-text {
      font-size: 24px;
      text-shadow: 0px 0px 1px black;
    }

    .app-name {
      font-size: 12px;
      text-shadow: 0px 0px 1px black;
    }

    .time-stamp {
      color: black;
    }

    &:only-child {
      border-radius: 5px;
      width: 100%;
      display: block;
      height: 100%;
    }

    &:not(:first-child) {
      border-radius: 0 5px 5px 0;
      height: 100%;
    }

    &:not(:last-child) {
      border-radius: 5px 0 0 5px;
      border-right: 1px solid black;
      height: 100%;
      vertical-align: top;
    }
  }
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity .5s;
}

.fade-enter,
.fade-leave-to

/* .fade-leave-active below version 2.1.8 */
  {
  opacity: 0;
}
</style>