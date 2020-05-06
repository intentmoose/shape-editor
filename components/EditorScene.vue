<template>
  <div class="EditorScene">
    <div class="buttons">
      <button class="button" role="button" v-for="btn in buttons" :key="btn" :title="btn.name" @click="currentTool = btn.name">
        <img class="inner-img" :src="btn.src" alt />
      </button>
    </div>

    <div class="edit-panel" v-if="currentTool !== ''">
      <div class="scale-tool" v-if="currentTool === 'scale'">
        <p class="title">scale:</p>
        <div class="tool-wrap">
          <div class="input-wrap">
            <div class="inner-wrap">
              <input
                class="pos-input"
                v-if="selectedObject.scale"
                v-model="selectedObject.scale.x"
                type="number"
              />
              <div class="btn-wrap">
                <button @click="selectedObject.scale.x++" class="add-sub-btn">+</button>
                <button @click="selectedObject.scale.x--" class="add-sub-btn">-</button>
              </div>
            </div>
            <label class="label">x</label>
          </div>
          <div class="input-wrap">
            <div class="inner-wrap">
              <input
                class="pos-input"
                v-if="selectedObject.scale"
                v-model="selectedObject.scale.y"
                type="number"
              />
              <div class="btn-wrap">
                <button @click="selectedObject.scale.y++" class="add-sub-btn">+</button>
                <button @click="selectedObject.scale.y--" class="add-sub-btn">-</button>
              </div>
            </div>
            <label class="label">y</label>
          </div>
          <div class="input-wrap">
            <div class="inner-wrap">
              <input
                class="pos-input"
                v-if="selectedObject.scale"
                v-model="selectedObject.scale.z"
                type="number"
              />
              <div class="btn-wrap">
                <button @click="selectedObject.scale.z++" class="add-sub-btn">+</button>
                <button @click="selectedObject.scale.z--" class="add-sub-btn">-</button>
              </div>
            </div>
            <label class="label">z</label>
          </div>
        </div>
      </div>
      <div class="position-tool" v-if="currentTool === 'position'">
        <p class="title">position:</p>
        <div class="tool-wrap">
          <div class="input-wrap">
            <div class="inner-wrap">
              <input
                class="pos-input"
                v-if="selectedObject.position"
                v-model="selectedObject.position.x"
                type="number"
              />
              <div class="btn-wrap">
                <button @click="selectedObject.position.x++" class="add-sub-btn">+</button>
                <button @click="selectedObject.position.x--" class="add-sub-btn">-</button>
              </div>
            </div>
            <label class="label">x</label>
          </div>
          <div class="input-wrap">
            <div class="inner-wrap">
              <input
                class="pos-input"
                v-if="selectedObject.position"
                v-model="selectedObject.position.y"
                type="number"
              />
              <div class="btn-wrap">
                <button @click="selectedObject.position.y++" class="add-sub-btn">+</button>
                <button @click="selectedObject.position.y--" class="add-sub-btn">-</button>
              </div>
            </div>
            <label class="label">y</label>
          </div>
          <div class="input-wrap">
            <div class="inner-wrap">
              <input
                class="pos-input"
                v-if="selectedObject.position"
                v-model="selectedObject.position.z"
                type="number"
              />
              <div class="btn-wrap">
                <button @click="selectedObject.position.z++" class="add-sub-btn">+</button>
                <button @click="selectedObject.position.z--" class="add-sub-btn">-</button>
              </div>
            </div>
            <label class="label">z</label>
          </div>
        </div>
      </div>
    </div>
    <a-scene vr-mode-ui="enabled: false" gridhelper="size: 200; divisions: 400" cursor="rayOrigin:mouse">

      <a-sky color="#20252c"></a-sky>
      <a-box
        @click="touchHandler"
        class="selected"
        position="0 0.5 0"
        rotation="0 0 0"
        color="#4CC3D9"
      >
        <a-box scale="1.05 1.05 1.05" model-opacity="0.2" color="yellow"></a-box>
      </a-box>

      <a-entity
        camera
        cursor="raycaster: mouse;"
        look-controls
        orbit-controls="target: 0 1.6 -0.5; minDistance: 0.5; maxDistance: 180; initialPosition: 0 5 15"
      ></a-entity>
    </a-scene>
  </div>
</template>

<script>
export default {
  name: "EditorScene",
  props: {
    msg: String
  },
  data: function() {
    return {
      currentTool: ''
    };
  },
  methods: {
    touchHandler() {
      console.log(event);
    }
  },
  computed: {
    sizeIcon() {
      return require("../assets/size.svg");
    },
    positionIcon() {
      return require("../assets/position.svg");
    },
    buttons() {
      return [
        { src: this.sizeIcon, name: "scale" },
        { src: this.positionIcon, name: "position" }
      ];
    },
    toolTypes() {
      return [
        {
          size: {
            x: this.selectedObject.scale.x,
            y: this.selectedObject.scale.y,
            z: this.selectedObject.scale.z
          },
          name: "scale"
        }
      ];
    }
  },
  mounted() {
    let isMoving = false;
    let el = document.querySelector(".selected");
    let object = el.object3D;
    let pos = object.position;
    this.selectedObject = object;
    console.log(object);

    // window.addEventListener("onmousedown", e => {
    //   // isMoving = true;
    //   console.log(e);
    // });

    // window.addEventListener(
    //   "touchstart",
    //   e => {
    //     if (this.arrowPressed !== "") {
    //       isMoving = true;
    //       // console.log("touchstart START");

    //       console.log(this.arrowPressed);
    //       // console.log(e.touches[0]);

    //       if (this.arrowPressed === "blue-arrow") {
    //         tStart = e.touches[0].clientY;
    //       }
    //       if (this.arrowPressed === "red-arrow") {
    //         tStart = e.touches[0].clientX;
    //       }
    //     }
    //   },
    //   { passive: true }
    // );
    // window.addEventListener(
    //   "touchmove",
    //   e => {
    //     // console.log("touchmove MOVE");

    //     // console.log(e);
    //     if (isMoving === true) {
    //       // console.log(arrowPressed);
    //       if (this.arrowPressed === "blue-arrow") {
    //         let tStartLocation = e.touches[0].clientY;

    //         if (tStartLocation < tStart) {
    //           pos.y += 0.05;
    //         }
    //         if (tStartLocation > tStart) {
    //           pos.y -= 0.05;
    //         }
    //       }
    //       if (this.arrowPressed === "red-arrow") {
    //         let tStartLocation = e.touches[0].clientX;

    //         if (tStartLocation > tStart) {
    //           pos.x += 0.05;
    //         }
    //         if (tStartLocation < tStart) {
    //           pos.x -= 0.05;
    //         }
    //       }
    //     }
    //     // backAndForth(0.7, tStartLocation, tStart);
    //   },
    //   { passive: true }
    // );
    // window.addEventListener("touchend", e => {
    //   if (isMoving === true) {
    //     console.log("touchend END");

    //     isMoving = false;
    //     this.arrowPressed = "";
    //   }
    // });

    AFRAME.registerComponent("model-opacity", {
      schema: { default: 1.0 },
      init: function() {
        this.el.addEventListener("model-loaded", this.update.bind(this));
      },
      update: function() {
        var mesh = this.el.getObject3D("mesh");
        var data = this.data;
        if (!mesh) {
          return;
        }
        mesh.traverse(function(node) {
          if (node.isMesh) {
            node.material.opacity = data;
            node.material.transparent = data < 1.0;
            node.material.needsUpdate = true;
          }
        });
      }
    });
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->

<style lang="scss">

.buttons {
  z-index: 10;
  position: absolute;
  left: 10px;
}
.button {
  width: 40px;
  height: 40px;
  margin-bottom: 5px;
  background-color: #495159bb;
  border: none;
  border-radius: 50%;
  color: white;
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 10;
  &:hover {
    opacity: 0.8;
    cursor: pointer;
  }
}

.add-sub-btn {
  color: #61afef;
  border: 2px #61afef solid;
  border-radius: 50%;
  width: 20px;
  height: 20px;
  display: flex;
  justify-content: center;
  align-items: center;
  background: none;
  &:hover {
    background: #61afef3a;
    cursor: pointer;
  }
}
.inner-img {
  width: 20px;
  height: 20px;
  color: black;
}
.edit-panel {
  position: absolute;
  padding: 10px;
  background-color: #495159e8;
  border-radius: 20px;
  border: 1px solid white;
  left: 50%;
  transform: translate(-50%, -50%);
  bottom: 5px;
  color: white;
  z-index: 5;
  p {
    margin: 0;
    padding: 0;
    margin-bottom: 10px;
  }
  .tool-wrap{
    display: flex;
  }
  .input-wrap {
    /* width: 40px; */
    display: flex;
    flex-direction: column;
    &:not(:last-of-type) {
      margin-right: 10px;
    }
    .inner-wrap {
      display: flex;
      .pos-input {
        height: 15px;
        align-self: center;
        margin-right: 4px;
        width: 40px;
      }
    }
    .label {
      text-align: center;
    }
  }
}

h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>

<style scoped>
</style>
