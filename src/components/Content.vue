<template>
  <h1>Ascii @ Amrita</h1>
  <p>
    Welcome! We’re the ASCII club of Amrita, and we host various technical and
    non-technical events and workshops.
  </p>
  <p>
    However, most importantly, the club regulates and enforces the usage of the
    various ASCII characters in and around the coimbatore campus. These
    characters can be seen in the table below.
  </p>
  <div class="table">
    <div class="col" v-for="characters in cols" :key="characters">
      <div>Dec</div>
      <div>Oct</div>
      <div>Hex</div>
      <div class="rt">C</div>
      <template v-for="charInfo in characters" :key="charInfo">
        <div class="rt">{{ charInfo.dec }}</div>
        <div class="rt">{{ charInfo.oct }}</div>
        <div class="rt">{{ charInfo.hex }}</div>
        <div class="rt">{{ charInfo.char }}</div>
      </template>
    </div>
  </div>
  <p>
    In order to use any one of these characters, please write to us at
    ascii@cb.amrita.edu. Note that this restriction does not apply to characters
    outside this range.
  </p>
  <p>
    &copy;2021 ASCII@Amrita<span class="notmobile">. All Rights Reserved.</span>
  </p>
</template>

<script>
import { ref, onMounted, onUnmounted } from "vue";

export default {
  name: "Content",
  setup() {
    const characters = [];

    for (let i = 0; i < 128; i++) {
      characters.push({
        dec: i.toString(),
        oct: i.toString(8),
        hex: i.toString(16),
        char: i >= 32 && i < 127 ? String.fromCharCode(i) : "NA",
      });
    }
    const cols = ref([characters]);
    const year = ref(6666);

    const setCols = () => {
      const pageWidth = document.body.clientWidth;
      const colCount = pageWidth >= 700 ? 4 : pageWidth >= 500 ? 3 : 2;
      const newCols = [];
      const rows = Math.ceil(characters.length / colCount);
      for (let i = 0; i < colCount; i++) {
        newCols.push(characters.slice(i * rows, (i + 1) * rows));
      }
      cols.value = newCols;
    };
    onMounted(() => {
      window.addEventListener("resize", setCols);
      year.value = new Date().getFullYear();
      setCols();
    });

    onUnmounted(() => {
      window.removeEventListener("resize", setCols);
    });

    return {
      cols,
    };
  },
};
</script>

<style scoped>
h1 {
  font-weight: 400;
  font-size: 72px;
  text-align: center;
}

p {
  max-width: min(700px, 95vw);
  text-align: center;
}

.table {
  display: flex;
  justify-content: space-evenly;
  align-self: stretch;
}
.col {
  display: grid;
  grid-template-columns: repeat(4, minmax(0, 1fr));
}
.rt {
  text-align: right;
}

@media screen and (max-width: 600px) {
  h1 {
    font-size: 48px;
  }
  .notmobile {
    display: none;
  }
}
</style>
