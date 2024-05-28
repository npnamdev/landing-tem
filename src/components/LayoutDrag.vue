<template>
  <div>
    <header>
      <div>logo</div>
      <button @click="toggleBox">Toggle Box</button>
    </header>
    <div class="wrapper-content" :class="{ 'content-expanded': isBoxHidden || isFloating }">
      <div class="content" :class="{ 'content-full': isBoxHidden && isFloating }">content</div>
      <div
        v-if="!isBoxHidden"
        :class="['box', { floating: isFloating }]"
        :style="boxStyle"
        @mousedown="startDrag"
      >
        <button @click.stop="toggleFloating">Toggle Floating</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
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
          height: "80vh",
          width: "250px",
          position: "absolute",
          cursor: "move",
          transform: "translateY(-50%)",
        };
      } else {
        return {
          top: "0px",
          right: "0px",
          height: "100vh",
          width: "250px",
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
#app {
  position: relative;
  height: 100vh;
  width: 100%;
  overflow: hidden;
  user-select: none;
}

.box {
  background-color: blue;
  
  color: white;
  padding: 20px;
}

.wrapper-content {
  display: grid;
  grid-template-columns: auto 250px;
  gap: 10px;
}

.content {
  background: red;
  border: 1px solid yellow;
  color: white;
  height: 100vh;
  padding: 20px;
}

.content-expanded {
  grid-template-columns: auto;
}

.content-full {
  grid-column: span 2;
}

header{
  height: 60px;
  background-color: darkorange;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0px 20px;
}
</style>
