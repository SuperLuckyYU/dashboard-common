<template>
  <i :style="iconStyle" class="image-icon" />
</template>

<script>
import { IMAGE_MSG } from './constants'

const sprites = require('./assets/sprites.png')
const unknow = require('./assets/unkonw.png')

export default {
  name: 'ImageIcon',
  props: {
    image: {
      type: String,
      required: true,
      validator: val => {
        if (Object.keys(IMAGE_MSG).includes(val.toLowerCase())) {
          return true
        } else {
          if (val !== 'other') {
            console.warn(`前端未集成${val}操作系统的icon`)
          }
          return true
        }
      },
    },
  },
  data () {
    return {
      sprites,
    }
  },
  computed: {
    imageInfo () {
      const image = this.image.toLowerCase()
      if (IMAGE_MSG[image]) {
        return {
          ...IMAGE_MSG[image],
          isUnknow: false,
        }
      } else {
        return {
          label: '未知',
          position: '0px 0px',
          isUnknow: true,
        }
      }
    },
    iconStyle () {
      const { isUnknow } = this.imageInfo
      const style = {
        width: '16px',
        height: '16px',
        display: 'inline-block',
        backgroundSize: 'cover',
        backgroundRepeat: 'no-repeat',
        backgroundImage: `url("${isUnknow ? unknow : sprites}")`,
        backgroundPosition: this.imageInfo.position,
      }
      return style
    },
  },
}
</script>
