<template>
  <div class="elevator-shaft">
    <div class="elevator" >
      <div :class="[ !settingsForPanelElevator.show ? 'opacity-zero' : '', 'panel' ]">
        <div v-show="settingsForPanelElevator.orientation === 'top'">
          &#9650;
        </div>
        <div v-show="settingsForPanelElevator.orientation === 'down'">
          &#9660;
        </div>

        <div>
          {{ settingsForPanelElevator.floorNumber }}
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ElevatorShaft',
  props: {
    settingsForPanelElevator: {
      type: Object,
      default: null
    }
  }
}

</script>

<style scoped>
.elevator-shaft {
  position: relative;
  width: 102px;
  border-right: 1px solid #bbbbbb;
  border-left: 1px solid #bbbbbb;
  margin-right: 4px;
}

.elevator {
  position: absolute;
  bottom: 0;
  left: 0;
  width: 100%;
  height: 100px;
  background-color: #a59a9a;
  transition: all;
}

.relax-animation {
  animation: 3s relax-animation;
}

@keyframes relax-animation {
    0%, 33.2%, 66.4%, 100% {
        opacity: 1;
    }
    16.6%, 49.8%, 83% {
        opacity: 0.5;
    }
}

.panel {
  display: flex;
  width: 48px;
  height: 22px;
  margin: 4px auto auto;
  background-color: #3f3d3d;
  opacity: 1;
  transition: .3s;
}

.opacity-zero {
  opacity: 0;
}

.panel > div {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 50%;
  color: #a59a9a;
  font-size: 14px;
  font-weight: 600;
}

</style>
