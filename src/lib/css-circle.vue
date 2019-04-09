<template>
<div class="v-circle" :style="circleStyle" >
  <div class="mask full" :style="[maskStyle, transitionStyle, rotateStyle]">
    <div class="fill" :style="[fillStyle, transitionStyle, rotateStyle]"></div>
  </div>
  <div class="mask half" :style="[maskStyle]">
    <div class="fill" :style="[fillStyle, transitionStyle, rotateStyle]"></div>
    <div class="fill fix" :style="[fillStyle, fixStyle, transitionStyle]"></div>
  </div>
  <div class="pv" :style="pvStyle">
    <span class="progress" :style="progressTextStyle">
      <span class="percent">{{ pv?pv:'' }}{{unit}}</span>
      <br>
      {{desc}}
    </span>
  </div>
</div>
</template>

<style lang="scss" scoped>
  .v-circle {
    border-radius: 50%;
    position: relative;
    box-sizing: content-box;
    .mask, .fill {
      position: absolute;
      border-radius: 50%;
      backface-visibility: hidden;
    }

    .pv {
      position: absolute;
      top: 0;
      left: 0;
      bottom: 0;
      right: 0;
      margin: auto;
      z-index: 1;
      border-radius: 50%;
      text-align: center;
      display: flex;
      align-items: center;
      .progress {
        margin: 0;
        padding: 0;
        font-family: 'Impact';
        font-size:14px;
        line-height:1.5;
        width:100%;
        .percent{
          font-size:1.6em;
        }
      }
    }

  }
</style>

<script>
const DEFAULT_WIDTH = 150
    , DEFAULT_BOLD = 5
    , DEFAULT_FONT_SIZE = 64
    , DEFAULT_BORDER_COLOR = '#bdc3c7'
    , DEFAULT_TEXT_COLOR = '#bdc3c7'
    , DEFAULT_FILL_COLOR = '#2ecc71'
    , DEFAUTL_BG_COLOR = '#f9f9f9'
    , DEFAUTL_TEXT_BG_COLOR = '#333333'

export default {
  name: 'v-circle',
  methods: {
    setClip(t, r, b, l) {
      return `rect(${t}px, ${r}px, ${b}px, ${l}px)`
    },
    setTransformStyle(pv, type) {
      let deg = Math.floor((pv / 100) * 180)
      if(type === 'fix') {
        // remove the gap between two half circles
        return `rotate(${deg * 2}deg)`
      }
      return `rotate(${deg}deg)`
    },
    setTransitionStyle(t) {
      return `transform ${t}s`
    },
    setPv() {
      let types = ['fix', 'rotate']
      // map styles
      types.map((type) => {
        this[type + 'Style'] = {
          transform: this.setTransformStyle(this.pv, type)
        }
      })
    }
  },

  props: [
    'color',
    'width',
    'fontSize',
    'pv',
    'textColor',
    'bold',
    'textBgColor',
    'borderColor',
    'during',
    'bgColor',
    'desc',
    'unit'
  ],
  computed:{
    transformStyleValue(){
      return this.setTransformStyle(this.pv)
    },
    innerCircleWidth(){
      return ((this.width || DEFAULT_WIDTH) - 2 * (this.bold || DEFAULT_BOLD)) + 'px'
    },
    fixTransformStyleValue(){
      return this.setTransformStyle(this.pv, 'fix')
    },
       transitionStyleValue(){
      return this.setTransitionStyle(this.during || 0.8)
    },
      // 环形样式
      circleStyle() {
        return {
        borderColor: (this.borderColor || DEFAULT_BORDER_COLOR),
        borderStyle: 'solid',
        borderWidth: this.bold/3 + 'px',
        width: (this.width || DEFAULT_WIDTH) + 'px',
        height: (this.width || DEFAULT_WIDTH) + 'px',
        backgroundColor: (this.bgColor || DEFAUTL_BG_COLOR)
        }
      },

      // 进度文字样式
      progressTextStyle() {
        return {
        fontSize: (this.fontSize || DEFAULT_FONT_SIZE) + 'px',
        color: this.textColor || DEFAULT_TEXT_COLOR
        }
      },

      fillStyle() {
        return {
        backgroundColor: this.color || DEFAULT_FILL_COLOR,
        width: (this.width || DEFAULT_WIDTH) + 'px',
        height: (this.width || DEFAULT_WIDTH) + 'px',
        clip: this.setClip(0, this.width / 2, this.width, 0)
        }
      },

      rotateStyle() {
        return {
        transform: this.transformStyleValue,
        webkitTransform: this.transformStyleValue,
        msTransform: this.transformStyleValue,
        oTransform: this.transformStyleValue,
        mozTransform: this.transformStyleValue
        }
      },

      transitionStyle() {
        return {
        transition: this.transitionStyleValue,
        webkitTransition: this.transitionStyleValue,
        mozTransition: this.transitionStyleValue,
        oTransition: this.transitionStyleValue,
        msTransition: this.transitionStyleValue
        }
      },

      maskStyle() {
        return {
        width: (this.width || DEFAULT_WIDTH) + 'px',
        height: (this.width || DEFAULT_WIDTH) + 'px',
        clip: this.setClip(0, this.width, this.width, this.width / 2)
        }
      },

      pvStyle() {
        return {
        backgroundColor: this.textBgColor || DEFAUTL_TEXT_BG_COLOR,
        width: this.innerCircleWidth,
        height: this.innerCircleWidth,
        lineHeight: this.innerCircleWidth
        }
      },

      fixStyle() {
        return {
        transform: this.fixTransformStyleValue,
        webkitTransform: this.fixTransformStyleValue,
        mozTransform: this.fixTransformStyleValue,
        oTransform: this.fixTransformStyleValue,
        msTransform: this.fixTransformStyleValue
        }
      }
  }
}
</script>
