<template>
  <div id="app">
    <b-container>
      <b-row>
        <b-col sm="6" offset-sm="3">
          <b-card class="mt-4">
            <b-row class="justify-content-between mx-2">
              <b-form-checkbox v-model="isOn" switch size="lg">
                Power
              </b-form-checkbox>
              <div class="border border-success rounded-lg px-4 py-2">
                {{ lastPlayed }}
              </div>
            </b-row>
            <b-row class="mx-2 mt-4 justify-content-center">
              <label for="volume">Volume</label>
              <b-form-input
                id="volume"
                v-model="volume"
                type="range"
                min="0"
                max="5"
              />
            </b-row>
            <b-row class="mx-2 mt-4 justify-content-between">
              <b-form-checkbox v-model="isLoop" switch size="lg">
                Loop
              </b-form-checkbox>
              <b-button variant="outline-info" @click="stop">
                Stop
              </b-button>
            </b-row>
          </b-card>
          <b-card class="my-4">
            <b-row class="justify-content-center">
              <div
                :class="[
                  'sound-button border rounded-lg m-1',
                  isOn ? 'border-warning' : 'button-disabled',
                ]"
                v-for="clip in clips"
                :key="clip.name"
                @click="playClip(clip)"
              >
                {{ clip.name }}
              </div>
            </b-row>
          </b-card>
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import clips from './clips';

export default {
  name: 'App',
  components: {},
  data() {
    return {
      clips,
      volume: 2,
      isOn: true,
      isLoop: false,
      lastPlayed: 'No Sound!',
      currentAudio: {},
    };
  },
  methods: {
    playClip({ name, url }) {
      if (this.isOn) {
        this.stop();
        const audio = new Audio(url);
        audio.volume = this.volume / 5;
        audio.loop = this.isLoop;
        audio.play();
        this.currentAudio = audio;
        this.lastPlayed = name;
      }
    },
    stop() {
      try {
        this.currentAudio.pause();
      } catch (err) {
        console.log(err);
      }
    },
  },
};
</script>

<style lang="scss">
.sound-button {
  width: 160px;
  height: 160px;
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: moccasin;
  &:hover {
    cursor: pointer;
  }
}
.button-disabled {
  border-color: #adb5bd !important;
  background-color: lightgray;
  &:hover {
    cursor: not-allowed;
  }
}
</style>
