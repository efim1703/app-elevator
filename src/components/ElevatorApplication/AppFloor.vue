<template>
  <div class="floor">
    <div class="number">
        {{ floorNumber }}.
    </div>
    <custom-button :buttonIsPressed="buttonIsPressed" @elevator-call="$emit('elevator-call', floorNumber)"/>
  </div>
</template>

<script>
import CustomButton from '@/components/ui/CustomButton.vue'

export default {
  name: 'AppFloor',
  components: {
    CustomButton
  },
  props: {
    floorNumber: {
        type: String,
        required: true
    },
    buttonIsPressed: {
        type: Boolean,
        default: false
    }
  }
}
</script>

<style scoped>
.floor {
    position: relative;
    height: 100px;
    width: 100%;
    border-bottom: 1px solid #bbbbbb;
}

.number {
    position: absolute;
    bottom: 50%;
    transform: translateY(50%);
    left: 28px;
    font-weight: 600;
}

</style>
