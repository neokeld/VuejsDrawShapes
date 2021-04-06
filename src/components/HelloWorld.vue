<template>
  <fragment>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <button v-on:click="addRectShape">Add Rect Shape</button>
    <svg
      viewBox="0 0 720 544"
      width="720"
      height="544"
      stroke="#000"
      fill="#000"
      strokeWidth="3"
      class="drawable"
    >
      <RectShape
        :x="rectShape.x"
        :y="rectShape.y"
        :w="rectShape.w"
        :h="rectShape.h"
        v-for="(rectShape, index) in rectShapes"
        :key="'rect' + index"
        @mousedown.native="onMouseDownRect($event, index)"
      />
      <template v-for="(rectShape, index) in rectShapes">
        <Arrow
          :p1="rectShape"
          :p2="rectShapes[index + 1]"
          v-if="rectShapes.length > index + 1"
          :key="index"
        />
      </template>
    </svg>
    <p>Click on the green button to add some shapes 🖌</p>
    <h2>Essential Links</h2>
    <ul>
      <li>
        <a
          href="https://vuejs.org"
          target="_blank"
        >
          Core Docs
        </a>
      </li>
      <li>
        <a
          href="https://forum.vuejs.org"
          target="_blank"
        >
          Forum
        </a>
      </li>
      <li>
        <a
          href="https://chat.vuejs.org"
          target="_blank"
        >
          Community Chat
        </a>
      </li>
      <li>
        <a
          href="https://twitter.com/vuejs"
          target="_blank"
        >
          Twitter
        </a>
      </li>
      <br>
      <li>
        <a
          href="http://vuejs-templates.github.io/webpack/"
          target="_blank"
        >
          Docs for This Template
        </a>
      </li>
    </ul>
    <h2>Ecosystem</h2>
    <ul>
      <li>
        <a
          href="http://router.vuejs.org/"
          target="_blank"
        >
          vue-router
        </a>
      </li>
      <li>
        <a
          href="http://vuex.vuejs.org/"
          target="_blank"
        >
          vuex
        </a>
      </li>
      <li>
        <a
          href="http://vue-loader.vuejs.org/"
          target="_blank"
        >
          vue-loader
        </a>
      </li>
      <li>
        <a
          href="https://github.com/vuejs/awesome-vue"
          target="_blank"
        >
          awesome-vue
        </a>
      </li>
    </ul>
  </div>
  </fragment>
</template>

<script>
import Arrow from './Arrow'
import RectShape from './RectShape'
import { Fragment } from 'vue-fragment'

export default {
  name: 'HelloWorld',
  components: {
    Arrow,
    Fragment,
    RectShape
  },
  data () {
    return {
      rectShapes: [],
      msg: 'Welcome to the Vue.js 2 Draw Shape App',
      positions: {
        clientX: undefined,
        clientY: undefined,
        index: undefined,
        movementX: 0,
        movementY: 0
      }
    }
  },
  methods: {
    addRectShape () {
      const maxW = 592
      const maxH = 416
      const min = 0
      const w = 64 * (Math.random() + 1)
      const h = 64 * (Math.random() + 1)
      const x = Math.random() * (maxW - min) + min
      const y = Math.random() * (maxH - min) + min
      this.rectShapes.push({x, y, w, h})
    },
    onMouseDownRect (event, index) {
      event.preventDefault()
      // get the mouse cursor position on mouse down:
      this.positions.clientX = event.clientX
      this.positions.clientY = event.clientY
      this.positions.index = index
      document.onmousemove = this.elementDrag
      document.onmouseup = this.closeDragElement
    },
    elementDrag (event) {
      event.preventDefault()
      this.positions.movementX = this.positions.clientX - event.clientX
      this.positions.movementY = this.positions.clientY - event.clientY
      this.positions.clientX = event.clientX
      this.positions.clientY = event.clientY
      // set the shape's new position:
      this.rectShapes[this.positions.index].x = this.rectShapes[this.positions.index].x - this.positions.movementX
      this.rectShapes[this.positions.index].y = this.rectShapes[this.positions.index].y - this.positions.movementY
    },
    closeDragElement () {
      document.onmouseup = null
      document.onmousemove = null
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
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
button {
  background-color: #42b983;
  border: none;
  color: #fff;
  cursor: pointer;
  display: inline-block;
  font-size: 24px;
  padding: 16px 32px;
  text-align: center;
  text-decoration: none;
}
.drawable {
  z-index: 2;
  position: absolute;
  top: 408px;
  left: 0;
  right: 0;
  margin: 0 auto;
}
</style>
