<template>
    <transition name="bounce">
      <div v-show="show"
      class="absolute bg-blue-lightest border-t-4 border-blue rounded-b text-blue-darkest px-4 py-3 shadow-md pointer-events-none"
      style="height: auto; min-height: 200%; top: 115%;  min-width:220px;" :style="this.xPosition" role="alert">
        <div class="flex">
             <font-awesome-icon class="mr-4 h-6 w-6 text-blue"  icon="info-circle"/>
          <div>
            <p class="font-bold text-xs mb-1">Tasks required to advance:</p>
            <ul class="text-left list-reset text-xs" v-for="(task, index) in this.blocking" :key="index">
              <li>{{task}}</li>
            </ul>
          </div>
        </div>
      </div>
    </transition>
</template>

<script>
export default {
  name: "tooltip",
  props: ["show", "blocking", "last"],
  computed: {
    xPosition() {
      if (this.last) {
        return { left: "0%" };
      } else {
        return { right: "0%" };
      }
    }
  }
};
</script>

<style>
.bounce-enter-active {
  animation: bounce-in 0.5s;
}
.bounce-leave-active {
  animation: bounce-in 0.5s reverse;
}
@keyframes bounce-in {
  0% {
    transform: scale(0);
  }
  50% {
    transform: scale(1.5);
  }
  100% {
    transform: scale(1);
  }
}
</style>
