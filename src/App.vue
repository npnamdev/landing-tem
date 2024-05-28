<template>
  <div class="namdev">
    <div class="layout">
      <header>
        <img src="./assets/logo.svg" class="logo" />
        <ul class="list-icon-1">
          <li>
            <Airplay stroke-width="1.5" />
          </li>
          <li>
            <CircleHelp stroke-width="1.5" />
          </li>
          <li>
            <AlignEndHorizontal stroke-width="1.5" />
          </li>
          <li>
            <Bug stroke-width="1.5" />
          </li>
          <li>
            <Book stroke-width="1.5" />
          </li>
          <li>
            <Settings stroke-width="1.5" />
          </li>
        </ul>
        <SlidersVertical
          @click="toggleBox"
          size="18"
          stroke-width="2"
          class="icon-hide-style"
          :class="isBoxHidden && 'active'"
        />
      </header>
      <div
        class="wrapper-content"
        :class="{ 'content-expanded': isBoxHidden || isFloating }"
      >
        <div
          class="content"
          :class="{ 'content-full': isBoxHidden && isFloating }"
        >
          content
        </div>
        <div
          v-if="!isBoxHidden"
          :class="['box', { floating: isFloating }]"
          :style="boxStyle"
          @mousedown="startDrag"
        >
          <Shrink @click.stop="toggleFloating" class="icon-shrink" :class="isFloating && 'active'"/>
          <div class="content-box">
            <h3>Section 1</h3>
          </div>
        </div>
      </div>
    </div>
    <LayoutLeft />
  </div>
</template>

<script>
import LayoutLeft from "./components/LayoutLeft.vue";
import {
  CircleHelp,
  Airplay,
  AlignEndHorizontal,
  Bug,
  Book,
  Settings,
  SlidersVertical,
  Shrink,
} from "lucide-vue-next";

export default {
  components: {
    LayoutLeft,
    CircleHelp,
    Airplay,
    AlignEndHorizontal,
    Bug,
    Book,
    Settings,
    SlidersVertical,
    Shrink,
  },
  data() {
    return {
      isFloating: false,
      isBoxHidden: false,
      position: { top: 0, right: 0 },
      dragStart: { x: 0, y: 0 },
      boxStart: { top: 0, right: 0 },
      initialPosition: { top: 0, right: 0 },
    };
  },
  computed: {
    boxStyle() {
      if (this.isFloating) {
        return {
          top: `${this.position.top}px`,
          right: `${this.position.right}px`,
          height: "75vh",
          width: "270px",
          position: "absolute",
          cursor: "move",
          transform: "translateY(-50%)",
        };
      } else {
        return {
          top: "0px",
          right: "0px",
          height: "100vh",
          width: "270px",
          position: "relative",
        };
      }
    },
  },
  methods: {
    toggleFloating() {
      if (this.isFloating) {
        this.position.top = this.initialPosition.top;
        this.position.right = this.initialPosition.right;
      } else {
        this.initialPosition.top = 0;
        this.initialPosition.right = 0;
        this.position.top = window.innerHeight / 2;
        this.position.right = 20;
      }
      this.isFloating = !this.isFloating;
    },
    toggleBox() {
      this.isBoxHidden = !this.isBoxHidden;
    },
    startDrag(event) {
      if (this.isFloating) {
        this.dragStart.x = event.clientX;
        this.dragStart.y = event.clientY;
        this.boxStart.top = this.position.top;
        this.boxStart.right = this.position.right;
        document.addEventListener("mousemove", this.onDrag);
        document.addEventListener("mouseup", this.stopDrag);
      }
    },
    onDrag(event) {
      const deltaX = this.dragStart.x - event.clientX;
      const deltaY = event.clientY - this.dragStart.y;
      this.position.top = this.boxStart.top + deltaY;
      this.position.right = this.boxStart.right + deltaX;
    },
    stopDrag() {
      document.removeEventListener("mousemove", this.onDrag);
      document.removeEventListener("mouseup", this.stopDrag);
    },
  },
};
</script>

<style scoped>
.box {
  background-color: rgb(255, 255, 255);
  color: white;
  padding: 20px;
  z-index: 1000;
}

.box.floating{
  border-radius: 5px;
  box-shadow: 0 -1px 38px rgba(0, 0, 0, 0.05), 0 4px 7px rgba(0, 0, 0, 0.12);
}

.wrapper-content {
  display: grid;
  grid-template-columns: auto 270px;
}

.content {
  color: white;
  height: calc(100vh - 55px);
  border-right: 1px solid #ebe4e4;
  padding: 20px;
  background-color: #fff;
}

.content-expanded {
  grid-template-columns: auto;
}

.content-full {
  grid-column: span 2;
}

header {
  height: 55px;
  background-color: rgb(255, 255, 255);
  display: flex;
  border-bottom: 1px solid #ebe4e4;
  align-items: center;
  justify-content: space-between;
  padding: 0px 20px;
}

.logo {
  width: 32px;
}

.list-icon-1 {
  margin: 0;
  display: flex;
  list-style: none;
  align-items: center;
  gap: 28px;
}

.list-icon-1 li {
  display: flex;
  align-items: center;
  cursor: pointer;
}

.list-icon-1 li:hover svg {
  color: #0881fc;
}

.list-icon-1 li svg {
  width: 19px;
  font-weight: 1px;
  stroke: 1px;
}

.icon-hide-style {
  cursor: pointer;
}

.icon-hide-style:hover {
  color: #0881fc;
}

.icon-hide-style.active {
  color: #0881fc;
}

.icon-shrink {
  color: rgb(119, 111, 111);
  width: 16px;
  position: absolute;
  right: 15px;
  top: 15px;
  cursor: pointer;
}

.icon-shrink:hover {
  color: #0881fc;
}

.icon-shrink.active {
  color: #0881fc;
}

.content-box {
  color: black;
}
</style>
