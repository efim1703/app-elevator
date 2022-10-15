<template>
  <div class="wrapper-button">
    <button 
        @click="$emit('elevator-call')"
        :class="buttonIsPressed ? 'pressed-color' : ''"
    />
  </div>
</template>

<script>
export default {
  name: 'CustomButton',
  props: {
    buttonIsPressed: {
        type: Boolean,
        required: true
    }
  }
}
</script>

<style scoped>
.wrapper-button {
    position: absolute;
    bottom: 50%;
    transform: translateY(50%);
    left: 52px;
    padding: 4px;
    opacity: 1;
}

.wrapper-button:hover {
    opacity: 0.7;
    cursor: pointer;
}

.wrapper-button:hover button {
    cursor: pointer;
}

button {
    width: 16px;
    height: 16px;
    background-color: #38be34;
    border: 1px solid #1a4619;
    opacity: 1;
}

.pressed-color {
    background-color: #d6c312; 
}

.indication-animation {
  animation: 2s indication-animation;
}

@keyframes indication-animation {
    0%, 100% {
        background-color: #d6c312;
    }
    50% {
        background-color: #be3030;
    }
}
</style>
