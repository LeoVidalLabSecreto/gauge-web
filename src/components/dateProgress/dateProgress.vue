<template>
  <div :style="{ width: width + 'px' }">
    <div :style="wrapperStyle">
      <div :style="actualStyle">
        <svg
          :width="width + 100"
          :height="height + 20"
          :viewBox="`-15 0 ${width + 100} ${height + 20}`"
        >
          <path
            :d="path"
            stroke="#191A37"
            :stroke-width="strokeWidth"
            fill="none"
            :stroke-dasharray="`${dashLength}, ${gapLength}`"
            transform="translate(35, 10)"
          />
          <circle cx="35" cy="140" r="6" fill="#191a37" stroke="white" :stroke-width="2" />
          <circle cx="204" cy="140" r="6" fill="#d3d3d3" stroke="white" :stroke-width="2" />

          <circle
            :cx="ballX + 35"
            :cy="ballY + 10"
            r="6"
            fill="#C93632"
            stroke="white"
            :stroke-width="2"
          />
          <text
            :x="ballX + 35 + labelOffsetX"
            :y="ballY - 5"
            font-size="20"
            fill="#C93632"
            text-anchor="middle"
            font-weight="500"
          >
            {{ formatDate(progress) }}
          </text>
        </svg>
      </div>
      <div :style="contentStyle">
        <div :style="dateIndicatorStyle">
          <text style="color: black"> {{ formatDate(startProgress) }} </text>
          <text :style="[{ color: 'black', textAlign: 'right' }]">
            {{ startProgress.toLocaleDateString('en', { weekday: 'short' }) }}
          </text>
        </div>
        <div :style="dateIndicatorStyle">
          <text style="color: black"> {{ formatDate(endProgress) }} </text>
          <text style="color: black">
            {{ endProgress.toLocaleDateString('en', { weekday: 'short' }) }}
          </text>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'DateProgress',
  props: {
    startProgress: {
      type: Date,
      required: true,
    },
    progress: {
      type: Date,
      required: true,
    },
    endProgress: {
      type: Date,
      required: true,
    },
  },
  computed: {
    strokeWidth() {
      return 2
    },
    width() {
      return 170
    },
    height() {
      return 130
    },
    radius() {
      return (this.width - this.strokeWidth) / 2
    },
    centerX() {
      return this.width / 2
    },
    centerY() {
      return this.height
    },
    dashLength() {
      return 4
    },
    gapLength() {
      return 4
    },
    totalDays() {
      return this.getDaysBetween(this.startProgress, this.endProgress)
    },
    progressDays() {
      return this.getDaysBetween(
        this.startProgress,
        this.progress > this.endProgress ? this.endProgress : this.progress,
      )
    },
    progressPercentage() {
      return (this.progressDays / this.totalDays) * 100
    },
    angle() {
      return (this.progressPercentage / 100) * 180
    },
    radians() {
      return ((180 - this.angle) * Math.PI) / 180
    },
    ballX() {
      return this.centerX + this.radius * Math.cos(this.radians)
    },
    ballY() {
      return this.centerY - this.radius * Math.sin(this.radians)
    },
    path() {
      return `M ${this.strokeWidth / 2}, ${this.centerY} A ${this.radius},${this.radius} 0 0,1 ${
        this.width - this.strokeWidth / 2
      },${this.centerY}`
    },
    labelOffsetX() {
      return this.progressPercentage < 25 ? -25 : this.progressPercentage > 75 ? 25 : 0
    },
    wrapperStyle() {
      return {
        height: '165px',
        width: '270px',
        // background: 'red',
        justifyContent: 'space-between',
        alignItems: 'center',
        display: 'flex',
        flexDirection: 'column',
      }
    },
    actualStyle() {
      return {
        // width: '56px',
        width: '270px',
        // height: '24px',
        justifyContent: 'center',
        alignItems: 'center',
        display: 'flex',
        // background: 'yellow',
      }
    },
    contentStyle() {
      return {
        width: '270px',
        height: '70px',
        flexDirection: 'row',
        justifyContent: 'space-between',
        display: 'flex',
        // background: 'blue',
        // position: 'absolute',
      }
    },
    dateIndicatorStyle() {
      return {
        marginTop: '6.5px',
      }
    },
    typographyStyle() {
      return {
        fontFamily: 'markProRegular',
        fontSize: '5px',
        color: 'secondary',
      }
    },
  },
  methods: {
    getDaysBetween(start, end) {
      const oneDay = 24 * 60 * 60 * 1000
      return Math.round((end.getTime() - start.getTime()) / oneDay)
    },
    formatDate(date) {
      const day = date.getUTCDate().toString().padStart(2, '0')
      const month = (date.getUTCMonth() + 1).toString().padStart(2, '0')
      return `${day}/${month}`
    },
  },
}
</script>

<style scoped>
/* Inline styles are already applied within the component using the :style directive */
</style>
