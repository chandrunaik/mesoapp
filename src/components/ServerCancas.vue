<template>
  <div>
    <h3>Server Canvas</h3>
    <div class="d-flex flex-wrap flex-row">
      <div v-for="(server, index) in servers" :key="index" class="server-container rounded m-3">
        <transition name="fade">
          <div class="app-container text-center" v-if="server.apps.length > 0">
            <!-- display apps -->
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
import * as moment from "moment";

export default {
  data() {
    return {
      servers: [
        {
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
    // add and destroy server event handler
    EventBus.$on("add-server", () => {
      this.addServer();
    });
    EventBus.$on("destroy-server", () => {
      this.destroyServer();
    });
    // add and remove app event handler
    EventBus.$on("add-app", appname => {
      this.addApp(appname);
    });
    EventBus.$on("remove-app", appname => {
      this.removeApp(appname);
    });
  },
  methods: {
    addServer() {
      let newServer = this.createServer();
      // add to servers list
      this.servers.push(newServer);
    },
    createServer() {
      // create dummy server object
      return {
        apps: []
      };
    },
    destroyServer() {
      // remove lastly added server
      if (this.servers.length > 0) {
        let server = this.servers.pop();
        // reallocate apps
        if (server.apps.length !== 0) {
          server.apps.forEach(app => {
            this.reallocateApp(app);
          });
        }
      } else {
        return;
      }
    },
    reallocateApp(app) {
      let index = this.findFreeServer();
      if (!index) {
        alert(`Removed ${app.appName} app due to no space in the cluster.`);
      } else {
        this.servers[index].apps.push(app);
      }
    },
    addApp(appname) {
      let index = this.findFreeServer();
      if (!index) {
        alert(`Please add new server to the cluster before adding new apps.`);
      } else {
        let newApp = this.createApp(appname);
        this.servers[index].apps.push(newApp);
      }
    },
    createApp(appname) {
      let app = {
        appName: appname,
        iconText: appname === "Hadoop" ? "Hd" : appname.substr(0, 2),
        timeStamp: moment().valueOf(),
        duration: moment().fromNow()
      };
      return app;
    },
    findFreeServer() {
      let index = undefined;

      //
      for (let i in this.servers) {
        if (this.servers[i].apps.length === 0) {
          index = i;
          break;
        }
      }

      if (!index) {
        for (let i in this.servers) {
          if (this.servers[i].apps.length === 1) {
            index = i;
            break;
          }
        }
      }

      return index;
    },
    removeApp(appname) {
      let minTime = undefined;
      let serverIndex = undefined;
      let appIndex = undefined;

      // find last added app using timestamp
      for (let i = 0; i <= this.servers.length - 1; i++) {
        for (let j = 0; j <= this.servers[i].apps.length - 1; j++) {
          if (this.servers[i].apps[j].appName === appname) {
            let timeStamp = Number(this.servers[i].apps[j].timeStamp);

            if (!minTime) {
              minTime = timeStamp;
              serverIndex = i;
              appIndex = j;
            } else if (minTime < timeStamp) {
              minTime = timeStamp;
              serverIndex = i;
              appIndex = j;
            }
          }
        }
      }

      if (minTime) {
        // remove last added app
        this.servers[serverIndex].apps.splice(appIndex, 1);
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
      background-image: linear-gradient(
        to left top,
        #9c00a7,
        #92039e,
        #880595,
        #7e068c,
        #740783
      );
    }

    &.Rails {
      background-image: linear-gradient(
        to left top,
        #0477a7,
        #007ba1,
        #007f98,
        #00818d,
        #078381
      );
    }

    &.Chronos {
      background-image: linear-gradient(
        to left top,
        #0043a7,
        #044bba,
        #0a53cc,
        #125ce0,
        #1964f3
      );
    }

    &.Storm {
      background-image: linear-gradient(
        to left top,
        #93a70f,
        #a5b31c,
        #b7c028,
        #c9cc33,
        #dcd93d
      );
    }

    &.Spark {
      background-image: linear-gradient(
        to right bottom,
        #00a745,
        #00b048,
        #00ba4b,
        #00c34d,
        #01cd50
      );
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
</style>
