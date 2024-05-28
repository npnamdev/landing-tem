<template>
  <div
    :class="['box-left']"
    :style="{ top: `${position.top}px`, left: `${position.left}px` }"
    @mousedown="startDrag"
  ></div>
</template>


<script>

export default {
  data() {
    return {
      position: { top: 0, left: 0 },
      isDragging: false,
      dragStart: { x: 0, y: 0 },
      boxStart: { top: 0, left: 0 },
      boxSize: { width: 45, height: 450 },
    };
  },
  mounted() {
    this.centerVertically();
    this.position.left = 20;
  },
  methods: {
    startDrag(event) {
      this.isDragging = true;
      this.dragStart.x = event.clientX;
      this.dragStart.y = event.clientY;
      this.boxStart.top = this.position.top;
      this.boxStart.left = this.position.left;
      document.addEventListener("mousemove", this.onDrag);
      document.addEventListener("mouseup", this.stopDrag);
    },
    onDrag(event) {
      if (this.isDragging) {
        const deltaX = event.clientX - this.dragStart.x;
        const deltaY = event.clientY - this.dragStart.y;
        let newLeft = this.boxStart.left + deltaX;
        let newTop = this.boxStart.top + deltaY;
        const viewportWidth = window.innerWidth;
        const viewportHeight = window.innerHeight;
        newLeft = Math.min(
          Math.max(newLeft, 0),
          viewportWidth - this.boxSize.width
        );
        newTop = Math.min(
          Math.max(newTop, 0),
          viewportHeight - this.boxSize.height
        );
        this.position.left = newLeft;
        this.position.top = newTop;
      }
    },
    stopDrag() {
      this.isDragging = false;
      document.removeEventListener("mousemove", this.onDrag);
      document.removeEventListener("mouseup", this.stopDrag);
    },
    centerVertically() {
      const viewportHeight = window.innerHeight;
      this.position.top = (viewportHeight - this.boxSize.height) / 2;
    },
  },
};
</script>

<style scoped>
.box-left {
  width: 45px;
  height: 450px;
  background-color: rgb(255, 255, 255);
  color: white;
  cursor: move;
  position: absolute;
  box-shadow: 0 -1px 38px rgba(0, 0, 0, .05), 0 4px 7px rgba(0, 0, 0, .12);
  border-radius: 4px;
  z-index: 50;
}
</style>
