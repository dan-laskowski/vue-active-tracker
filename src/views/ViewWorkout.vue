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
