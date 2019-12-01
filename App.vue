<template>
  <div>
    <p class="info">FPS: {{ fps }}</p>
    <div class="controls">
      Amount
      <select @change="updateCount">
        <option value="20" selected>20</option>
        <option value="100">100</option>
        <option value="200">200</option>
        <option value="500">500</option>
        <option value="1000">1000</option>
        <option value="2000">2000</option>
        <option value="10000">10000</option>
      </select>
    </div>
    <a class="repo" href="https://github.com/IgorHalfeld/3d-sphere-vue">
      https://github.com/IgorHalfeld/3d-sphere-vue
    </a>
    <main ref="element"></main>
  </div>
</template>

<script>
import { WebGlHelper } from './WebGl';

const random = () => Math.random() * 360;
const getRotations = (num = 20) => Array(num).fill().map(() => [random(), random(), random()]);

export default {
  name: 'SphereVue',
  data: () => ({
    renderer: Object.create(null),
    fps: 0,
    count: 20,
    rotations: getRotations(),
  }),
  created() {
    const handleAnimation = () => {
      const { element } = this.$refs
      this.renderer = new WebGlHelper({
        container: element,
        onFPSUpdate: fps => this.fps = fps,
      });

      this.renderer.animate();
      this.renderer.syncBoxes(this.rotations);
    };


    this.$nextTick(handleAnimation);
  },
  methods: {
    updateCount(event) {
      this.count = event.target.value;
      this.rotations = getRotations(Number(this.count));
      this.renderer.syncBoxes(this.rotations);
    }
  }
};
</script>

<style>
html,body{
  margin: 0;
  padding: 0;
}
main {
  width: 100%;
  height: 100%;
}
.controls {
  position: fixed;
  top: 1em;
  right: 1em;
}
.info {
  position: fixed;
  top: 1em;
  left: 1em;
}
.repo {
  position: fixed;
  bottom: 1em;
  left: 1em;
}

p {
  margin-top: 0;
}
</style>