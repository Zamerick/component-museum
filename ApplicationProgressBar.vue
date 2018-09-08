<docs>
Application Progress Bar Component
This component is a prototype for a feature request at work. Applicatons have multiple stages,
with a series of tasks that must be completed before the user can progress to the next stage.
This component provides visual feedback to the user about where they are, and the progress they
are making in completing the overall application. This component uses builtin dummy data to demonstrate
how it would function to end users. It also includes some buttons to change state,that would not be present in the finished component.
</docs>

<template>
  <div class="w-full mx-auto root" :style="this.backgroundImage" >
    <!-- Header Text -->
    <h1 class="text-center text-5xl text-white pt-16 mb-32 font-display font-light">{{currentStage.message}}</h1>

    <!-- Stepper Container -->
    <div class="flex justify-between mt-32 mb-8 mx-16">
      <div class="flex justify-between" :class="[isLastStage(index) ? 'flex-1' : '']" v-for="(stage, index) in stages" :key="index">
        <div
          @mouseover="toggleShow(index)"
          @mouseleave="toggleShow(index)"
          :class="[isLastStage(index) ? 'lg:w-12 sm:w-10' : 'lg:w-10 sm:w-8', isCompletedStage(index) ? 'bg-white border-grey' : 'border-green bg-green']"
          class="relative md:h-8 lg:h-10 border-2 border-grey text-center rounded-full">
          <font-awesome-icon
            :class="[isCompletedStage(index) ? 'text-grey' : 'text-white']"
            class="pointer-events-none"
            size="lg"
            :icon="isCompletedStage(index) ? stage.icon : 'check'"/>
          <Tooltip :show="isNextStage(index) ? showToolTip : false" :blocking="blocking" :last="isLastStage(index)"></Tooltip>
        </div>
        <progress-bar v-if="isLastStage(index)" :total="totalBlockingTasks" :completed="completedBlockingTasks" :tense="tense(index)"/>
      </div>
    </div>

    <!-- Test Controls -->
    <div class="test-controls pin-t pin-r absolute align-middle bg-ghost rounded-l-lg mt-4 p-2 cursor-pointer">
      <div class="flex flex-1" @click="advanceStage()">
        <span class="w-8 h-8 border-2 border-transparent text-center bg-orange block rounded-full">
          <font-awesome-icon class="text-white " icon="plus"/>
        </span>
        <span class="ml-3 mt-2 h-8 align-middle">
          Next Stage
        </span>
      </div>
      <div class="flex flex-1 " @click="completeTask()" >
        <span class="test-control w-8 h-8 text-center bg-orange block rounded-full">
          <font-awesome-icon class="text-white test-button" icon="plus"/>
        </span>
        <span class="ml-3 mt-2 align-middle">
          Complete Task
        </span>
      </div>

    </div>
  </div>
</template>

<script>
import ProgressBar from '@/components/ProgressBar'
import Tooltip from '@/components/Tooltip'
export default {
  name: 'home',
  components: {
    ProgressBar,
    Tooltip
  },
  computed: {
    currentStage() {
      return this.stages[this.applicationStatus - 1]
    },
    backgroundImage() {
      let imageString =
        "linear-gradient(rgba(20, 20, 20, 0.6), rgba(20, 20, 20, 0.6)), url('/images/" +
        this.currentStage.image +
        "')"
      return {
        background: imageString,
        height: '50%',
        'background-position': 'center',
        'background-repeat': 'no-repeat',
        'background-size': 'cover'
      }
    }
  },
  methods: {
    isCompletedStage(index) {
      return this.applicationStatus <= index
    },
    isLastStage(index) {
      return index != this.stages.length - 1
    },
    isNextStage(index) {
      return index == this.applicationStatus
    },

    tense(index) {
      if (index < this.applicationStatus - 1) {
        return 'past'
      } else if (index == this.applicationStatus - 1) {
        return 'present'
      } else if (index > this.applicationStatus - 1) {
        return 'future'
      }
    },
    advanceStage() {
      this.applicationStatus = this.applicationStatus + 1
      this.completedBlockingTasks = 0
    },
    completeTask() {
      this.completedBlockingTasks = this.completedBlockingTasks + 1
    },
    toggleShow(index) {
      if (this.isNextStage(index)) {
        this.showToolTip = !this.showToolTip
      }
    }
  },
  data: () => {
    return {
      stages: [
        {
          name: 'New',
          image: 'adventure-climb-climber-306531.jpg',
          message: 'Welcome!',
          icon: 'pencil-alt'
        },
        {
          name: 'Submitted',
          image: 'asphalt-black-and-white-clouds-715221.jpg',
          message: 'Your Application has been submitted!',
          icon: 'envelope'
        },
        {
          name: 'In-Review',
          image: 'balconies-beach-blue-water-1167021.jpg',
          message:
            "We're reviewing your application. Someone will contact you soon!",
          icon: 'search'
        },
        {
          name: 'Accepted',
          image: 'beach-cliff-clouds-1168742.jpg',
          message: 'Congradulations, Your application has been approved!',
          icon: 'plane-departure'
        },
        {
          name: 'Confirmed',
          image: 'business-dark-evening-1164675.jpg',
          message: "You've been confirmed. Time to pack your bags!",
          icon: 'plane'
        },
        {
          name: 'Arrived',
          image: 'camera-clouds-daylight-840666.jpg',
          message: "You've Arrived! Let's get settled and start learning!",
          icon: 'plane-arrival'
        },
        {
          name: 'Completed',
          image: 'landscape-mountains-nature-15382.jpg',
          message: "Yay! you've completed the program. You're Awesome!",
          icon: 'graduation-cap'
        }
      ],
      blocking: [
        'Passport',
        'Academic Data',
        'Transcript',
        'Application Fee',
        'Leo Dicaprio winning an oscar',
        'something freezing over',
        'World Peace'
      ],
      totalBlockingTasks: 7,
      completedBlockingTasks: 0,
      isCurrentStage: true,
      applicationStatus: 1,
      showToolTip: false
    }
  }
}
</script>
<style>
svg {
  position: relative;
  left: 1px;
  top: 6px;
}
.test-control svg {
  left: 0px;
  top: 7px;
}
</style>
