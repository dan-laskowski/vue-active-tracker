<template>
  <div class="max-w-screen-md mx-auto px-4 py-10">
    <!--- Status message -->
    <div
      v-if="statusMsg || errorMsg"
      class="mb-10 p-4 bg-light-grey rounded-md"
    >
      <p class="text-at-light-green">{{ statusMsg }}</p>
      <p class="text-red-500">{{ errorMsg }}</p>
    </div>

    <!--- Create -->
    <div class="p-8 flex items-start bg-light-grey rounded-md shadow-lg">
      <form
        @submit.prevent="createWorkout"
        class="flex flex-col gap-y-5 w-full"
      >
        <h1 class="text-2xl text-at-light-green">Record Workout</h1>

        <!-- Workout Name -->
        <div class="flex flex-col">
          <label class="mb-1 text-sm text-at-light-green" for="workout-name"
            >Workout Name</label
          >
          <input
            id="workout-name"
            class="p-2 text-gray-500 focus:outline-none"
            type="text"
            v-model="workoutName"
            required
          />
        </div>

        <!-- Workout Type -->
        <div class="flex flex-col">
          <label class="mb-1 text-sm text-at-light-green" for="workout-type"
            >Workout Type</label
          >
          <select
            @change="workoutChange"
            class="p-2 text-gray-500 focus:outline-none"
            name="workout-type"
            id="workout-type"
            v-model="workoutType"
            required
          >
            <option value="select-workout" disabled>Select Workout</option>
            <option value="strength">Strength Training</option>
            <option value="cardio">Cardio</option>
          </select>
        </div>
        <!-- Strength Training Inputs -->
        <div class="flex flex-col gap-y-4" v-if="workoutType === 'strength'">
          <div
            class="flex flex-col gap-x-6 gap-y-2 relative md:flex-row"
            v-for="(item, index) in exercises"
            :key="index"
          >
            <div class="flex flex-col md:w-1/3">
              <label
                class="mb-1 text-sm text-at-light-green"
                for="exercise-name"
                >Exercise</label
              >
              <input
                class="p-2 w-full text-grey-500 focus:outline-none"
                type="text"
                v-model="item.exercise"
                required
              />
            </div>

            <div class="flex flex-col flex-1">
              <label class="mb-1 text-sm text-at-light-green" for="sets"
                >Sets</label
              >
              <input
                class="p-2 w-full text-grey-500 focus:outline-none"
                type="text"
                v-model="item.sets"
                required
              />
            </div>

            <div class="flex flex-col flex-1">
              <label class="mb-1 text-sm text-at-light-green" for="reps"
                >Reps</label
              >
              <input
                class="p-2 w-full text-grey-500 focus:outline-none"
                type="text"
                v-model="item.reps"
                required
              />
            </div>

            <div class="flex flex-col flex-1">
              <label class="mb-1 text-sm text-at-light-green" for="weight"
                >Weight (LB's)</label
              >
              <input
                class="p-2 w-full text-grey-500 focus:outline-none"
                type="text"
                v-model="item.weight"
                required
              />
            </div>
            <img
              @click="deleteExercise(item.id)"
              class="h-4 w-auto absolute -left-5 cursor-pointer"
              src="../assets/images/trash-light-green.png"
              alt=""
            />
          </div>
          <button
            @click="addExercise"
            type="button"
            class="
              mt-6
              py-2
              px-6
              rounded-sm
              self-start
              text-sm text-white
              bg-at-light-green
              duration-200
              border-solid border-2 border-transparent
              hover:border-at-light-green
              hover:bg-white
              hover:text-at-light-green
            "
          >
            Add exercise
          </button>
        </div>

        <!-- Cardio Training Inputs-->
        <div class="flex flex-col gap-y-4" v-if="workoutType === 'cardio'">
          <div
            class="flex flex-col gap-x-6 gap-y-2 relative md:flex-row"
            v-for="(item, index) in exercises"
            :key="index"
          >
            <div class="flex flex-col md:w-1/3">
              <label
                class="mb-1 text-sm text-at-light-green"
                for="exercise-name"
                >Type</label
              >
              <select
                class="p-2 w-full text-gray-500 focus:outline-none"
                id="cardio-type"
                v-model="item.cardioType"
              >
                <option value="#" disabled>Select Type</option>
                <option value="run" disabled>Run</option>
                <option value="walk" disabled>Walk</option>
              </select>
            </div>

            <div class="flex flex-col flex-1">
              <label class="mb-1 text-sm text-at-light-green" for="distance"
                >Distance</label
              >
              <input
                class="p-2 w-full text-grey-500 focus:outline-none"
                type="text"
                v-model="item.distance"
                required
              />
            </div>

            <div class="flex flex-col flex-1">
              <label class="mb-1 text-sm text-at-light-green" for="duration"
                >Duration</label
              >
              <input
                class="p-2 w-full text-grey-500 focus:outline-none"
                type="text"
                v-model="item.duration"
                required
              />
            </div>

            <div class="flex flex-col flex-1">
              <label class="mb-1 text-sm text-at-light-green" for="pace"
                >Pace</label
              >
              <input
                class="p-2 w-full text-grey-500 focus:outline-none"
                type="text"
                v-model="item.pace"
                required
              />
            </div>
            <img
              @click="deleteExercise(item.id)"
              class="h-4 w-auto absolute -left-5 cursor-pointer"
              src="../assets/images/trash-light-green.png"
              alt=""
            />
          </div>
          <button
            @click="addExercise"
            type="button"
            class="
              mt-6
              py-2
              px-6
              rounded-sm
              self-start
              text-sm text-white
              bg-at-light-green
              duration-200
              border-solid border-2 border-transparent
              hover:border-at-light-green
              hover:bg-white
              hover:text-at-light-green
            "
          >
            Add exercise
          </button>
        </div>
        <button
          type="submit"
          class="
            mt-6
            py-2
            px-6
            rounded-sm
            self-start
            text-sm text-white
            bg-at-light-green
            duration-200
            border-solid border-2 border-transparent
            hover:border-at-light-green hover:bg-white hover:text-at-light-green
          "
        >
          Record workout
        </button>
      </form>
    </div>
  </div>
</template>

<script>
import { ref } from "vue";
import { uid } from "uid";
import { supabase } from "../supabase/init";
export default {
  name: "Create",
  components: {},
  setup() {
    const workoutName = ref("");
    const workoutType = ref("select-workout");
    const exercises = ref([]);
    const statusMsg = ref(null);
    const errorMsg = ref(null);

    const addExercise = () => {
      if (workoutType.value === "strength") {
        exercises.value.push({
          id: uid(),
          exercises: "",
          sets: "",
          reps: "",
          weight: "",
        });
        return;
      }
      exercises.value.push({
        id: uid(),
        cardioType: "",
        distance: "",
        duration: "",
        pace: "",
      });
    };
    const deleteExercise = (id) => {
      if (exercises.value.length > 1) {
        exercises.value = exercises.value.filter(
          (exercise) => exercise.id !== id
        );
        return;
      }
      errorMsg.value =
        "Error: Cannot remove, need to at least have one exercise";
      setTimeout(() => {
        errorMsg.value = false;
      }, 5000);
    };
    const workoutChange = () => {
      exercises.value = [];
      addExercise();
    };
    const createWorkout = async () => {
      try {
        const { error } = await supabase.from("workouts").insert([
          {
            workoutName: workoutName.value,
            workoutType: workoutType.value,
            exercises: exercises.value,
          },
        ]);
        if (error) throw error;
        statusMsg.value = "Success: Workout created!";
        workoutName.value = null;
        workoutType.value = "select-workout";
        exercises.value = [];
        setTimeout(() => {
          statusMsg.value = false;
        }, 5000);
      } catch (error) {
        errorMsg.value = `Error: ${error.message}`;
        setTimeout(() => {
          errorMsg.value = false;
        }, 5000);
      }
    };

    return {
      workoutName,
      workoutType,
      exercises,
      statusMsg,
      errorMsg,
      addExercise,
      workoutChange,
      deleteExercise,
      createWorkout,
    };
  },
};
</script>
