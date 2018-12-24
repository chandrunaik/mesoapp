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
    <div id="available-apps" class="mt-3">
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
    border-left: 4px solid #92039e;

    .fa-plus-circle {
      color: #92039e;
    }
  }

  &.Rails {
    border-left: 4px solid #007f98;

    .fa-plus-circle {
      color: #007f98;
    }
  }

  &.Chronos {
    border-left: 4px solid #044bba;

    .fa-plus-circle {
      color: #044bba;
    }
  }

  &.Storm {
    border-left: 4px solid #a5b31c;

    .fa-plus-circle {
      color: #a5b31c;
    }
  }

  &.Spark {
    border-left: 4px solid #00b048;

    .fa-plus-circle {
      color: #00b048;
    }
  }
  padding: 15px 20px;
  background-color: #141414;
  border-bottom: 1px solid #4a4a4a;
  margin: 0px;
  border-left: 2px solid red;
}
</style>
