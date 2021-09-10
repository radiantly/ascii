<template>
  <Content />
  <div class="dark-overlay" ref="overlay">
    <div class="middle">
      <Content />
    </div>
  </div>
</template>

<script>
import Content from "./components/Content.vue";
import { ref, onMounted, onUnmounted } from "vue";
import gsap from "gsap";

export default {
  name: "App",
  setup() {
    const overlay = ref(null);
    let clipRight = 0;
    const setClip = (e) => {
      clipRight = Math.max(0, document.body.clientWidth - e.clientX);
      gsap.to(overlay.value, {
        clipPath: `inset(0 ${clipRight}px 0 0)`,
      });
    };

    const setClipOut = () => {
      const bodyWidth = document.body.clientWidth;
      if (clipRight > 200 && bodyWidth - clipRight > 200) return;
      gsap.to(overlay.value, {
        clipPath: `inset(0 ${clipRight > bodyWidth / 2 ? bodyWidth : 0}px 0 0)`,
      });
    };

    onMounted(() => {
      window.addEventListener("mousemove", setClip);
      document.body.addEventListener("mouseleave", setClipOut);
    });
    onUnmounted(() => {
      window.removeEventListener("mousemove", setClip);
      document.body.removeEventListener("mouseleave", setClipOut);
    });
    return {
      overlay,
    };
  },
  components: {
    Content,
  },
};
</script>

<style>
@font-face {
  font-family: "Bohemian Typewriter";
  src: url("assets/Bohemian Typewriter.woff2");
}

*,
::before,
::after {
  box-sizing: border-box;
}

html,
body {
  margin: 0;
  min-height: 100vh;
}

body {
  font-family: "Bohemian Typewriter";
  display: flex;
  justify-content: center;
  font-size: 20px;
  color: #333;
  background-color: #fff;
  position: relative;
}

#app,
.middle {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.dark-overlay {
  background-color: #222;
  width: 100%;
  position: absolute;
  top: 0;
  left: 0;
  color: #fff;
  display: flex;
  justify-content: center;
  clip-path: inset(0 50vw 0 0);
  pointer-events: none;
}

h1 {
  color: #444;
}

.dark-overlay h1 {
  color: #fff;
}

.table {
  color: #404040;
}

.dark-overlay .table {
  color: #efefef;
}

@media screen and (max-width: 600px) {
  body {
    font-size: 18px;
    user-select: none;
  }
}
</style>
