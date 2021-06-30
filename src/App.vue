<template>
  <nav>
    <h1>
      <img alt="logo" src="./assets/logo.png" />
      <span>Command</span>
    </h1>
    <div class="hint">
      <svg
        t="1624894116412"
        class="icon"
        viewBox="0 0 1024 1024"
        version="1.1"
        xmlns="http://www.w3.org/2000/svg"
        p-id="688"
        width="40"
        height="40"
      >
        <path
          d="M646.6048 847.3088H377.3952c-39.4752 0-71.5776-32.1024-71.5776-71.5776v-54.016c-103.9872-71.0144-163.2256-187.5968-159.0784-314.112 6.2464-189.0816 159.1296-344.0128 348.0576-352.7168 101.0688-4.6592 196.7104 31.1296 269.6192 100.7104 72.9088 69.632 113.1008 163.5328 113.1008 264.3456 0 121.4464-59.3408 233.4208-159.2832 301.7216v54.016c0 39.5264-32.1536 71.6288-71.6288 71.6288zM512.1024 115.9168c-4.8128 0-9.6768 0.1024-14.5408 0.3584C340.48 123.4944 213.3504 252.3136 208.128 409.6c-3.584 108.4416 48.8448 208.128 140.288 266.6496a40.4736 40.4736 0 0 1 18.7904 34.2528v65.2288c0 5.5808 4.5568 10.1376 10.1376 10.1376h269.2096c5.5808 0 10.1376-4.5568 10.1376-10.1376v-65.2288c0-13.9264 7.0144-26.7264 18.7904-34.2528 87.9616-56.2688 140.4416-152.064 140.4416-256.256 0-83.8656-33.4336-161.9456-94.1056-219.904-56.9856-54.6304-131.0208-84.1728-209.7152-84.1728z m196.608 612.0448s-0.0512 0.0512 0 0c-0.0512 0.0512 0 0 0 0zM668.16 954.1632H355.7376c-16.9472 0-30.72-13.7728-30.72-30.72s13.7728-30.72 30.72-30.72h312.4224c16.9472 0 30.72 13.7728 30.72 30.72s-13.7728 30.72-30.72 30.72z"
          fill="#565B6D"
          p-id="689"
        ></path>
        <path
          d="M643.1232 605.1328c-8.4992 0-16.9984-3.5328-23.04-10.4448a30.72 30.72 0 0 1 2.7648-43.3664c101.1712-89.088 51.5584-206.8992 51.0464-208.0256a30.76608 30.76608 0 0 1 15.9232-40.448c15.5648-6.7584 33.6384 0.3584 40.448 15.9232 23.9104 54.9888 40.7552 184.0128-66.816 278.6816a30.5408 30.5408 0 0 1-20.3264 7.68z"
          fill="#FF9813"
          p-id="690"
        ></path>
      </svg>
    </div>
  </nav>
  <main>
    <div class="handle" :style="{ width: endWidth + 'px' }" ref="root">
      <div class="content">hi</div>
      <div class="separator" @mousedown="startDrag"><i></i><i></i></div>
    </div>
    <div class="desktop">
      <div class="content">Welcome</div>
    </div>
  </main>
</template>

<script setup lang="ts" >
import { ref, onMounted } from "vue";
const root = ref(" ");
let startX: number, startWidth: number;

const getHandleDivWidth = () => {
  // @ts-ignore
  return root.value.clientWidth;
};
startWidth =
  Number(localStorage.getItem("handle_width")) || getHandleDivWidth();
let endWidth = ref<number>(startWidth);

onMounted(() => {
  //@ts-ignore
  console.dir(root.value.clientWidth);
  console.log(root.value);
});

const startDrag = (e: MouseEvent): void => {
  e.preventDefault();
  startX = e.clientX;
  startWidth = getHandleDivWidth();

  document.onmousemove = (e) => {
    onDrag(e);
  };
  document.onmouseup = (e) => {
    stopDrag(e);
  };
};
const onDrag = (e: MouseEvent) => {
  let newWidth = startWidth + e.clientX - startX;
  endWidth.value = newWidth;
};
const stopDrag = (e: MouseEvent): void => {
  console.log("3333");

  localStorage.setItem("handle_width", String(getHandleDivWidth()));

  document.onmousemove = null;
  document.onmouseup = null;
};
</script>

<style lang="scss">
nav {
  border-bottom: 1px solid #aaa;
  box-shadow: 0px 1px 4px rgba(0, 0, 0, 0.3);
  display: flex;
  padding: 0 1em;
  justify-content: space-between;
  min-height: 10vh;
  img {
    width: 80px;
  }
}

h1,
.hint {
  position: relative;
}
span,
svg {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
}
svg {
  right: 0;
}
main {
  display: flex;
  min-height: 90vh;
}

.handle {
  background-color: #eee;
  position: relative;
  min-width: 200px;
}
.desktop {
  flex: 1;
}
.content {
  padding: 20px;
}
.handle .separator {
  display: flex;
  justify-content: center;
  align-items: center;
  position: absolute;
  top: 0;
  right: 0;
  width: 14px;
  height: 100%;
  background-color: white;
  box-shadow: 0px 0px 2px rgba(0, 0, 0, 0.35);
  cursor: col-resize;
}
.handle .separator i {
  display: inline-block;
  height: 14px;
  width: 1px;
  background-color: #e9e9e9;
  margin: 0 1px;
}
.handle .content {
  padding-right: 34px;
}
</style>
