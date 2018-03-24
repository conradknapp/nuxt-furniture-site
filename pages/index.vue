<template>
  <div class="container">
    <main>
      <nav><h1>vermillion<span id="light">home</span></h1>
      <nuxt-link to="/product"><i class="material-icons">subject</i></nuxt-link>
      </nav>
      <div id="description">
       <transition-group name="fade">
        <h1 :key="number" v-for="number in [currentNumber]">{{products[Math.abs(currentNumber) % products.length].title}}</h1>
        </transition-group>
        <p>{{products[Math.abs(currentNumber) % products.length].description}}</p>
      </div>
       <div id="scroll-buttons" @mouseenter="stopRotation" @mouseleave="startRotation">
         <a href="#"><button @click="prev"><i class="material-icons">keyboard_arrow_left</i></button></a>
        <a href="#"><button @click="next"><i class="material-icons">keyboard_arrow_right</i></button></a>
      </div> 
    </main>
    <transition name="fade-2">
    <section :key="number" v-for="number in [currentNumber]" :style="{ backgroundImage: `url(${products[Math.abs(currentNumber) % products.length].homeImageUrl})`}"></section>
    </transition>
    <aside :style="{ backgroundColor: products[Math.abs(currentNumber) % products.length].bgColor}">
      <h5 class="aside-title">START EXPLORING</h5>
       <transition-group name="list">
      <h2 :key="number" v-for="number in [currentNumber]">Explore {{products[Math.abs(currentNumber) % products.length].category}} &#x27f6;</h2>
      </transition-group>
    </aside>
    <aside>
      <h5 class="aside-title">FEATURED PRODUCT</h5>
       <transition-group name="list">
      <img :key="number" v-for="number in [currentNumber]" :src="products[Math.abs(currentNumber) % products.length].singleImageUrl" alt="">
      </transition-group>
    </aside>
  </div>
</template>

<script>
export default {
  data() {
    return {
      products: [
        {
          title: "Spacious kitchen",
          description:
            "Lorem ipsum dolor sit amet, consectetur adipisicing elit. Voluptatem beatae minima.",
          singleImageUrl:
            "https://st.hzcdn.com/simgs/db7126da077411da_9-6633/home-design.jpg",
          homeImageUrl:
            "https://st.hzcdn.com/simgs/cec10a4805820207_4-7893/contemporary-kitchen.jpg",
          category: "barstools",
          bgColor: "rgb(225, 111, 23)"
        },
        {
          title: "Big island",
          description:
            "Voluptatem beatae minima, esse eos vero ex sapiente dolorum quae hic omnis provident perspiciatis molestias recusandae.",
          singleImageUrl:
            "https://st.hzcdn.com/simgs/8b31c370037d24f8_9-0395/contemporary-home-decor.jpg",
          homeImageUrl:
            "https://st.hzcdn.com/simgs/1d21e9dd0ff5c6a8_9-0787/rustic-kitchen.jpg",
          category: "lights",
          bgColor: "#9fcaac"
        }
      ],
      currentNumber: 0,
      timer: null
    };
  },
  created() {
    this.startRotation();
  },
  methods: {
    startRotation() {
      this.timer = setInterval(this.next, 10000);
    },
    stopRotation() {
      clearTimeout(this.timer);
      this.timer = null;
    },
    next() {
      this.currentNumber += 1;
    },
    prev() {
      this.currentNumber -= 1;
    }
  }
};
</script>

<style>
body {
  background: #000;
}

.container {
  font-family: "Poppins", Arial, sans-serif;
  display: grid;
  grid-template-areas:
    "main section aside-1"
    "main section aside-2";
  grid-template-columns: 4fr 4fr 3fr;
  grid-template-rows: 1fr 1fr;
  height: 100vh;
  overflow-x: hidden;
  overflow-y: hidden;
}

.fade-enter-active {
  animation: 0.3s acrossIn 0.5s ease-in both;
}

.fade-leave-active {
  animation: 0.3s acrossOut 0.5s ease-in both;
}

@keyframes acrossIn {
  0% {
    transform: translate3d(30%, 0, 0);
    opacity: 0;
  }
  70% {
    opacity: 0.4;
  }
  100% {
    transform: translate3d(0, 0, 0);
    opacity: 1;
  }
}

@keyframes acrossOut {
  0% {
    transform: translate3d(0%, 0, 0);
    opacity: 1;
  }
  100% {
    transform: translate3d(-100%, 0, 0);
    opacity: 0;
  }
}

.fade-2-enter-active {
  animation: acrossIn-2 1.5s cubic-bezier(0.25, 0.142, 0.165, 1) both;
}

.fade-2-leave-active {
  animation: acrossOut-2 1.5s cubic-bezier(0.25, 0.1422, 0.165, 1) both;
}

@keyframes acrossIn-2 {
  0% {
    /* position: relative; */
    height: 100vh;
    transform: translate3d(100%, 0, -2px);
  }
  100% {
    transform: perspective(10px) translate3d(0, 0, 0px);
  }
}

@keyframes acrossOut-2 {
  0% {
    /* position: relative; */
  }
  100% {
    transform: perspective(80px) translate3d(-100%, 0, -1px);
  }
}

.list-enter-active {
  transition: all 1s ease-in-out 0.8s;
}

.list-leave-active {
  transition: all 1s ease-in-out 0.8s;
  position: absolute;
}

.list-enter,
.list-leave-to {
  opacity: 0;
  transform: translateX(-30px);
}

button {
  outline: 0;
}

a:not(button) {
  color: orangered;
}

main {
  grid-area: main;
  background-color: teal;
  color: white;
  display: grid;
  grid-template-areas:
    "nav"
    "desc"
    "buttons";
  grid-template-rows: 3fr 4fr 2fr;
}

main nav {
  display: flex;
  flex: 1;
  align-items: baseline;
  justify-content: space-between;
  margin: 1em;
}

main nav h1 {
  color: rgb(250, 64, 64);
  font-size: 2em;
}

#light {
  font-family: "Helvetica";
  font-style: italic;
  font-weight: 200;
}

nav {
  grid-area: nav;
}

#description h1 {
  position: absolute;
  width: 360px;
}

#description p {
  padding-top: 120px;
  position: relative;
}

#description {
  width: 350px;
  padding: 0 4em;
  grid-area: desc;
}

#scroll-buttons {
  grid-area: buttons;
  margin-left: 3em;
}

#scroll-buttons button {
  border: 1px solid #9fcaac;
  border-radius: 100%;
  margin: 0 1em;
  padding: 1em;
  cursor: pointer;
}

#scroll-buttons button:hover {
  animation: pulse 2s forwards;
}

#scroll-buttons button:active {
  outline: none;
  filter: drop-shadow(0 3px 12px rgba(0, 0, 0, 0));
  transform: scale(0.95);
  transition: all 0.1s ease-in;
}

#description h1 {
  line-height: 1.2;
  font-size: 3.5em;
}

#description p {
  line-height: 2;
  font-size: 0.9em;
}

#description > * {
  margin-top: 2em;
}

section {
  z-index: -10;
  background-size: cover;
  background-position: bottom;
  background-repeat: no-repeat;
  grid-area: section;
}

aside:first-of-type {
  grid-area: aside-1;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  padding: 3em;
  background: white;
}

.aside-title {
  letter-spacing: 3px;
  font-weight: 200;
}

aside:first-of-type h2 {
  line-height: 1.4;
}

aside:nth-of-type(2) {
  background: white;
  text-align: center;
  grid-area: aside-2;
  padding: 3em;
  display: flex;
  justify-content: space-between;
  flex-direction: column;
  align-items: center;
}

aside:nth-of-type(2) img {
  max-height: 200px;
}

@keyframes pulse {
  0% {
    box-shadow: 0 0;
  }
  70% {
    box-shadow: 0 0 5px 10px rgba(238, 11, 11, 0);
  }
  100% {
    box-shadow: 0 0 0 0 rgba(231, 10, 10, 0);
  }
}
</style>
