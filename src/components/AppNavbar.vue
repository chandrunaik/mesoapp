<template>
  <div>
    <div class="mt-5 mb-5 d-flex justify-content-center" id="server-action">
      <div class="mr-2 text-center pointer" @click="addServer">
        <div><i class="fas fa-plus-circle fa-2x"></i></div>
        <div class="mt-1">Add Server</div>
      </div>
      <div class="ml-2 text-center pointer" @click="destroyServer">
        <div><i class="fas fa-minus-circle fa-2x"></i></div>
        <div class="mt-1">Destroy</div>
      </div>
    </div>
    <div id="available-apps">
      <p class="pl-3 mb-1">Available Apps</p>
      <ul class="list-group d-flex flex-column">
        <li
          v-for="app in availableApps"
          :key="app.name"
          class="list-group-item d-flex justify-content-between"
          :class="app.name"
        >
          <div class="flex">{{ app.name }}</div>
          <div>
            <span @click="removeApp(app.name)" class="mr-2 pointer"
              ><i class="fas fa-minus-circle fa-lg"></i
            ></span>
            <span @click="addApp(app.name)" class="ml-2 pointer"
              ><i class="fas fa-plus-circle fa-lg"></i
            ></span>
          </div>
        </li>
      </ul>
    </div>
  </div>
</template>
<script>
import { EventBus } from "./../event-bus.js";

export default {
  data() {
    return {
      availableApps: [
        { name: "Hadoop" },
        { name: "Rails" },
        { name: "Chronos" },
        { name: "Storm"},
        { name: "Spark"}
      ]
    };
  },
  methods: {
    removeApp(appName) {
      EventBus.$emit("remove-app", appName);
    },
    addApp(appName) {
      EventBus.$emit("add-app", appName);
    },
    addServer() {
      EventBus.$emit("add-server");
    },
    destroyServer() {
      EventBus.$emit("destroy-server");
    }
  }
};
</script>
<style lang="less" scoped>
.pointer {
  cursor: pointer;
}

.list-group-item {
  &.Hadoop {
    border-left: 2px solid violet;

    .fa-plus-circle {
      color: violet;
    }
  }

  &.Rails {
    border-left: 2px solid green;

    .fa-plus-circle {
      color: green;
    }
  }

  &.Chronos {
    border-left: 2px solid blue;

    .fa-plus-circle {
      color: blue;
    }
  }

  &.Storm {
    border-left: 2px solid yellow;

    .fa-plus-circle {
      color: yellow;
    }
  }

  &.Spark {
    border-left: 2px solid green;

    .fa-plus-circle {
      color: green;
    }
  }
  padding: 15px 20px;
  background-color: #141414;
  border-bottom: 1px solid #4a4a4a;
  margin: 0px;
  border-left: 2px solid red;
}
</style>
