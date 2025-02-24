<template lang="pug">
.device
  a.title(:href="baseUri + device.link") {{ device.description }}
  .thumb
    .top-right
      div
        .vendor {{ device.vendor }}
          q-icon(name="more" v-if="device.isWhiteLabel")
            q-tooltip(self="center end") White-Label Device
    .is-new(v-if="isNew") new
      q-tooltip(self="center start") Added at {{ new Date(device.addedAt).toLocaleString() }}
    a.device-img(
      :href="baseUri + device.link"
      :style="{ backgroundImage: 'url(' + baseUri + device.image + ')' }"
    )
  .desc
    .model {{ device.model }}
    | - {{ device.exposes.join(', ') }}
</template>

<script>
import { ref, watch } from "vue";

export default {
  name: 'Device',
  props: ['device'],

  setup(props) {
    let baseUri = './';
    if(process.env.NODE_ENV !== 'production') {
      baseUri = 'https://www.zigbee2mqtt.io/';
    }

    const isNew = ref(false);
    watch(props.device, device => {
      isNew.value = new Date(device.addedAt) > Date.now() - 30 * 24 * 3600 * 1000;
    }, { immediate: true });

    return {
      isNew,
      baseUri
    }
  }
}
</script>

<style lang="scss">

$borderColor: #939b9d;
$accentColor: #1E6BB8;
$bgColor: #F2F2F2;

.device {
  position: relative;
  display: flex;
  height: 280px;
  flex-direction: column;
  box-shadow: 2px 2px 10px 0 rgba(0, 0, 0, 0.3);
  background: #fff;
  margin-top: 15px !important;
  border: 1px solid $borderColor;
  width: 345px;
  z-index: 3;
  backface-visibility: hidden;
  transform-origin: 10% 50%;

  .title {
    color: #4f5558;
    background: $bgColor;
    padding: 0.5rem 0.5rem;
    border-bottom: 1px solid $borderColor;
    font-weight: bold;
    font-size: 0.9rem;
    text-align: center;
    display: block;
    transition: color 0.1s ease-in-out;
    text-decoration: none;

    &:hover {
      color: $accentColor;
    }
  }

  .desc {
    padding: 0.3rem 0.5rem;
    font-size: 12px;
  }

  .model {
    padding-right: 5px;
    display: inline-block;
    color: #159957;
    font-weight: bold;
  }

  .thumb {
    font-size: 0;
    position: relative;
    border-bottom: 1px solid $borderColor;
    flex-grow: 1;

    .device-img {
      width: 100%;
      height: 100%;
      display: inline-block;
      background-repeat: no-repeat;
      background-size: contain;
      background-position: center center;
    }

    .top-right {
      position: absolute;
      right: 0;
      top: 0;
      text-align: right;
      .vendor {
        display: inline-block;
        padding: 0.2rem 0.5rem;
        background: $bgColor;
        color: #159957;
        font-size: 14px;
        font-weight: bold;
        border-bottom-left-radius: 6px;
      }

      .q-icon {
        font-size: 20px;
        vertical-align: middle;
        margin-left: 5px;
      }
    }

    .is-new {
      position: absolute;
      left: 0;
      top: 0;
      padding: 0.2rem 0.5rem;
      background: $bgColor;
      border-bottom-left-radius: 6px;
      font-size: 16px;
      font-weight: bold;
      border-bottom-right-radius: 6px;
      color: #165d93;
    }

  }
}
</style>
