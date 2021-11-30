<template>
  <div class="max-w-screen-sm mx-auto px-4 py-10">
    <!-- App Message -->
    <div
      class="mb-10 p-4 rounded-md shadow-md bg-light-green"
      v-if="statusMsg || errorMsg"
    >
      <p class="text-at-light-green">
        {{ statusMsg }}
      </p>
      <p class="text-red-500">
        {{ errorMsg }}
      </p>
    </div>
    <div v-if="dataLoaded">
      <!-- General workout information -->
      <div
        class="
          flex flex-col
          items-center
          p-8
          rounded-md
          shadow-md
          bg-light-grey
          relative
        "
      >
        <div class="flex absolute left-2 top-2 gap-x-2" v-if="user">
          <div
            class="
              h-7
              w-7
              rounded-full
              flex
              justify-center
              items-center
              cursor-pointer
              bg-at-light-green
              shadow-lg
            "
            @click="editMode"
          >
            <img
              class="h-3.5 w-auto"
              src="../assets/images/pencil-light.png"
              alt=""
            />
          </div>

          <div
            class="
              h-7
              w-7
              rounded-full
              flex
              justify-center
              items-center
              cursor-pointer
              bg-at-light-green
              shadow-lg
            "
          >
            <img
              class="h-3.5 w-auto"
              src="../assets/images/trash-light.png"
              alt=""
            />
          </div>
        </div>
        <img
          v-if="data.workoutType === 'cardio'"
          class="h-24 w-auto"
          src="../assets/images/running-light-green.png"
          alt=""
        />
        <img
          v-else
          class="h-24 w-auto"
          src="../assets/images/dumbbell-light-green.png"
          alt=""
        />

        <span
          class="
            mt-6
            py-1.5
            px-5
            text-xs text-white
            bg-at-light-green
            rounded-lg
            shadow-md
          "
          >{{ data.workoutType }}</span
        >
        <div class="w-full mt-6">
          <input
            v-if="edit"
            type="text"
            class="p-2 w-full text-grey-500 focus:outline-none"
            v-model="data.workoutName"
          />
          <h1 v-else class="text-at-light-green text-2xl text-center">
            {{ data.workoutName }}
          </h1>
        </div>
      </div>
    </div>
    <!-- Exercises information -->
    <div
      class="
        mt-10
        p-8
        rounded-md
        flex flex-col
        item-center
        bg-light-grey
        shadow-md
      "
    >
      <!-- Strength training-->
      <div
        v-if="data.workoutType === 'strength'"
        class="flex flex-col gap-y-4 w-full"
      >
        <div
          class="flex flex-col gap-x-6 gap-y-2 relative sm:flex-row"
          v-for="(item, index) in data.exercises"
          :key="index"
        >
          <div class="flex flex-2 flex-col md:w-1/3">
            <label for="exercise-name" class="mb-1 text-sm text-at-light-green">
              Exercise</label
            >
            <input
              id="exercise-name"
              type="text"
              v-if="edit"
              class="p-2 w-full text-gray-500 focus:outline-none"
              v-model="item.exercise"
            />
            <p v-else>{{ item.exercise }}</p>
          </div>
          <div class="flex flex-1 flex-col">
            <label for="sets" class="mb-1 text-sm text-at-light-green">
              Sets</label
            >
            <input
              id="sets"
              type="text"
              v-if="edit"
              class="p-2 w-full text-gray-500 focus:outline-none"
              v-model="item.sets"
            />
            <p v-else>{{ item.sets }}</p>
          </div>
          <div class="flex flex-1 flex-col">
            <label for="reps" class="mb-1 text-sm text-at-light-green">
              Reps</label
            >
            <input
              id="reps"
              type="text"
              v-if="edit"
              class="p-2 w-full text-gray-500 focus:outline-none"
              v-model="item.reps"
            />
            <p v-else>{{ item.reps }}</p>
          </div>
          <div class="flex flex-1 flex-col">
            <label for="weight" class="mb-1 text-sm text-at-light-green">
              Weight</label
            >
            <input
              id="weight"
              type="text"
              v-if="edit"
              class="p-2 w-full text-gray-500 focus:outline-none"
              v-model="item.weight"
            />
            <p v-else>{{ item.weight }}</p>
          </div>
          <img
            v-if="edit"
            class="absolute h-4 w-auto -left-5 cursor-pointer"
            src="../assets/images/trash-light-green.png"
            alt=""
          />
        </div>
        <button
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
          Add exercise
        </button>
      </div>

      <!-- Cardio training-->
      <div v-else class="flex flex-col gap-y-4 w-full">
        <div
          class="flex flex-col gap-x-6 gap-y-2 relative sm:flex-row"
          v-for="(item, index) in data.exercises"
          :key="index"
        >
          <div class="flex flex-2 flex-col md:w-1/3">
            <label for="cardio-type" class="mb-1 text-sm text-at-light-green">
              Type</label
            >
            <select
              class="p-2 w-full text-gray-500 focus:outline-none"
              v-if="edit"
              id="cardio-type"
            >
              <option value="#" disabled>Select type</option>
              <option value="run">Run</option>
              <option value="walk">Walk</option>
            </select>
            <p v-else>{{ item.cardioType }}</p>
          </div>
          <div class="flex flex-1 flex-col">
            <label for="distance" class="mb-1 text-sm text-at-light-green">
              Distance</label
            >
            <input
              id="distance"
              type="text"
              v-if="edit"
              class="p-2 w-full text-gray-500 focus:outline-none"
              v-model="item.distance"
            />
            <p v-else>{{ item.distance }}</p>
          </div>
          <div class="flex flex-1 flex-col">
            <label for="duration" class="mb-1 text-sm text-at-light-green">
              Duration</label
            >
            <input
              id="duration"
              type="text"
              v-if="edit"
              class="p-2 w-full text-gray-500 focus:outline-none"
              v-model="item.duration"
            />
            <p v-else>{{ item.duration }}</p>
          </div>
          <div class="flex flex-1 flex-col">
            <label for="pace" class="mb-1 text-sm text-at-light-green">
              Pace</label
            >
            <input
              id="pace"
              type="text"
              v-if="edit"
              class="p-2 w-full text-gray-500 focus:outline-none"
              v-model="item.pace"
            />
            <p v-else>{{ item.pace }}</p>
          </div>
          <img
            v-if="edit"
            class="absolute h-4 w-auto -left-5 cursor-pointer"
            src="../assets/images/trash-light-green.png"
            alt=""
          />
        </div>
        <button
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
          Add exercise
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import { ref, computed } from "vue";
import { supabase } from "../supabase/init";
import { useRoute } from "vue-router";
import store from "../store/index";
export default {
  name: "ViewWorkout",
  components: {},
  setup() {
    const data = ref(null);
    const dataLoaded = ref(null);
    const errorMsg = ref(null);
    const statusMsg = ref(null);
    const route = useRoute();
    const user = computed(() => store.state.user);

    const currentId = route.params.workoutId;

    const getData = async () => {
      try {
        const { data: workouts, error } = await supabase
          .from("workouts")
          .select("*")
          .eq("id", currentId);
        if (error) throw error;
        data.value = workouts[0];
        dataLoaded.value = true;
      } catch (error) {
        errorMsg.value = error.message;
        setTimeout(() => {
          errorMsg.value = false;
        }, 5000);
      }
    };

    const edit = ref(null);

    const editMode = () => {
      edit.value = !edit.value;
    };

    getData();

    return { data, dataLoaded, statusMsg, edit, editMode, user };
  },
};
</script>
