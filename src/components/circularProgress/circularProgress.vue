<template>
  <div :style="containerStyle">
    <div :style="wrapperStyle">
      <svg :width="width" :height="height" viewBox="0 -10 125 125" fill="none">
        <defs>
          <linearGradient
            id="paint0_linear"
            x1="-7.4"
            y1="83.1"
            x2="151.715"
            y2="26.5899"
            gradientUnits="userSpaceOnUse"
          >
            <stop offset="0.25" stop-color="#A9AAC8" />
            <stop offset="0.68" stop-color="#191A37" />
          </linearGradient>
        </defs>
        <path
          d="M95.4153 101.459C95.6108 101.691 95.1219 101.11 95.2509 101.252C99.0186 105.422 107.358 105.409 111.113 101.228C111.242 101.085 109.81 102.799 110.382 102.114C116.191 95.1565 120.437 86.9813 122.772 78.138C126.055 65.7017 125.396 52.5553 120.886 40.5082C116.375 28.4611 108.235 18.1053 97.5854 10.8662C86.9357 3.62713 74.3 -0.139525 61.4174 0.0846465C48.5347 0.308818 36.0383 4.5128 25.6479 12.118C15.2575 19.7232 7.48383 30.3559 3.39681 42.5526C-0.690202 54.7493 -0.889707 67.9106 2.82576 80.2252C5.46787 88.9823 9.99734 97.005 16.0456 103.756C16.6421 104.422 15.1508 102.757 15.2841 102.896C19.1826 106.944 27.5179 106.666 31.138 102.367C31.2618 102.22 30.7927 102.82 30.9803 102.58C31.6413 101.735 31.9717 101.312 32.0564 101.198C35.4086 96.6694 35.4627 93.4236 32.2633 88.7858C32.1824 88.6686 31.4941 87.7257 30.1175 85.8399C27.4746 82.2192 25.4441 78.1597 24.1347 73.8197C21.7459 65.9022 21.8741 57.4404 24.5018 49.5987C27.1295 41.757 32.1274 34.9209 38.8078 30.0312C45.4882 25.1415 53.5226 22.4387 61.8053 22.2945C70.088 22.1504 78.2119 24.5721 85.059 29.2264C91.906 33.8807 97.1396 40.5387 100.04 48.2842C102.939 56.0298 103.363 64.482 101.252 72.4778C100.095 76.8602 98.2082 80.9873 95.6937 84.6974C94.3832 86.6311 93.7279 87.598 93.6511 87.7182C90.6159 92.4647 90.7832 95.7055 94.2908 100.114C94.3796 100.226 94.7248 100.637 95.4153 101.459Z"
          fill="url(#paint0_linear)"
        />
        <circle
          :cx="ballX"
          :cy="ballY"
          r="19.5"
          fill="#262744"
          stroke="white"
          stroke-width="3.89"
        />

        <text
          :x="ballX"
          :y="ballY + 5"
          text-anchor="middle"
          font-size="20"
          fill="white"
          font-weight="bold"
        >
          {{ progress }}
        </text>
      </svg>
    </div>
    <div :style="contentStyle">
      <text> {{ startProgress }}</text>
      <text> {{ endProgress }}</text>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'

export default defineComponent({
  name: 'CircularProgress',
  props: {
    progress: {
      type: Number,
      required: true,
    },
    startProgress: {
      type: Number,
      required: true,
    },
    endProgress: {
      type: Number,
      required: true,
    },
  },
  computed: {
    strokeWidth() {
      return 12
    },
    width() {
      return 125
    },
    height() {
      return 106
    },
    normalizedProgress() {
      return (Math.min(this.progress, this.endProgress) / this.endProgress) * 100
    },
    ballX() {
      return this.getCenterX(this.normalizedProgress) + this.radius * Math.cos(this.radians)
    },
    ballY() {
      return this.getCenterY(this.normalizedProgress) + this.radius * Math.sin(this.radians)
    },
    angle() {
      const totalArcDegrees = 282
      const adjustedProgress = (this.normalizedProgress / 100) * totalArcDegrees
      const angleOffset = 130
      return adjustedProgress + angleOffset
    },
    radians() {
      return (this.angle * Math.PI) / 180
    },
    radius() {
      return (this.width - this.strokeWidth) / 2
    },
    containerStyle() {
      return {
        display: 'flex',
        justifyContent: 'center',
        alignItems: 'center',
        flexDirection: 'column',
        padding: '20px',
        marginTop: '-90px',
      }
    },
    wrapperStyle() {
      return {
        display: 'flex',
        justifyContent: 'center',
        alignItems: 'center',
      }
    },
    contentStyle() {
      return {
        display: 'flex',
        justifyContent: 'space-between',
        width: '50%',
        marginTop: '10px',

        color: '#262744',
      }
    },
  },
  methods: {
    getCenterY(progress: number) {
      if (progress <= 40) {
        return 50 + progress / 2
      }
      if (progress <= 60) {
        return 70
      }
      if (progress <= 80) {
        return 70 - (progress - 60) / 2
      }
      return 50
    },
    getCenterX(progress: number) {
      if (progress <= 20) {
        return 60 + progress / 2
      }
      if (progress <= 40) {
        return 70 - (progress - 20) / 2
      }
      if (progress <= 60) {
        return 60
      }
      if (progress <= 80) {
        return 60 - (progress - 60) / 3
      }
      return 60 + (progress - 80) / 10
    },
  },
})
</script>

<style scoped></style>
