<script setup>
import { PulseLoader } from "vue-spinner";
import { reactive, onMounted } from "vue";
import { useRoute, useRouter, RouterLink } from "vue-router";
import { useToast } from "vue-toastification";
import axios from "axios";
import BackButton from "@/components/BackButton.vue";
import NotFoundView from "./NotFoundView.vue";

const route = useRoute();
const router = useRouter();
const toast = useToast();
const jobId = route.params.id;
const state = reactive({
  job: {},
  isLoading: true,
  notFound: false,
});

const deleteJob = async () => {
  try {
    const confirm = window.confirm("Are you sure you want to delete this job?");
    if (confirm) {
      await axios.delete(
        `https://69d4dad7d396bd74235dc85b.mockapi.io/api/jobs/${jobId}`,
      );
      toast.success("Job deleted successfully");
      router.push("/jobs");
    }
  } catch (error) {
    console.error("Error deleting job: ", error);
    toast.error("Job couldn't be deleted");
  }
};

onMounted(async () => {
  try {
    const response = await axios.get(
      `https://69d4dad7d396bd74235dc85b.mockapi.io/api/jobs/${jobId}`,
    );
    if (!response.data || !response.data.id) {
      state.notFound = true;
      return;
    }
    state.job = response.data;
  } catch (e) {
    console.log("Error fetching job: ", e);
    state.notFound = true;
  } finally {
    state.isLoading = false;
  }
});
</script>

<template>
  <BackButton />
  <NotFoundView
    v-if="state.notFound"
    prevPage="jobs"
    material="job"
    Button="View Jobs"
  />
  <div v-else-if="state.isLoading" class="loader-parent">
    <div class="loader"></div>
  </div>
  <section v-else class="bg-green-50">
    <div class="container m-auto py-10 px-6">
      <div class="grid grid-cols-1 md:grid-cols-70/30 w-full gap-6">
        <main>
          <div
            class="bg-white p-6 rounded-lg shadow-md text-center md:text-left"
          >
            <div class="text-gray-500 mb-4">{{ state.job.type }}</div>
            <h1 class="text-3xl font-bold mb-4">{{ state.job.title }}</h1>
            <div
              class="text-gray-500 mb-4 flex align-middle justify-center md:justify-start"
            >
              <i class="pi pi-map-marker text-xl text-orange-700 mr-2"></i>
              <p class="text-orange-700">{{ state.job.location }}</p>
            </div>
          </div>

          <div class="bg-white p-6 rounded-lg shadow-md mt-6">
            <h3 class="text-green-800 text-lg font-bold mb-6">
              Job Description
            </h3>

            <p class="mb-4">
              {{ state.job.description }}
            </p>

            <h3 class="text-green-800 text-lg font-bold mb-2">Salary</h3>

            <p class="mb-4">{{ state.job.salary }} / Year</p>
          </div>
        </main>

        <!-- Sidebar -->
        <aside>
          <!-- Company Info -->
          <div class="bg-white p-6 rounded-lg shadow-md">
            <h3 class="text-xl font-bold mb-6">Company Info</h3>

            <h2 class="text-2xl">{{ state.job.company.name }}</h2>

            <p class="my-2">
              {{ state.job.company.description }}
            </p>

            <hr class="my-4" />

            <h3 class="text-xl">Contact Email:</h3>

            <p class="my-2 bg-green-100 p-2 font-bold">
              {{ state.job.company.contactEmail }}
            </p>

            <h3 class="text-xl">Contact Phone:</h3>

            <p class="my-2 bg-green-100 p-2 font-bold">
              {{ state.job.company.contactPhone }}
            </p>
          </div>

          <!-- Manage -->
          <div class="bg-white p-6 rounded-lg shadow-md mt-6">
            <h3 class="text-xl font-bold mb-6">Manage Job</h3>
            <RouterLink
              :to="`/jobs/edit/${state.job.id}`"
              class="bg-green-500 hover:bg-green-600 text-white text-center font-bold py-2 px-4 rounded-full w-full focus:outline-none focus:shadow-outline mt-4 block"
              >Edit Job</RouterLink
            >
            <button
              @click="deleteJob"
              class="bg-red-500 hover:bg-red-600 text-white font-bold py-2 px-4 rounded-full w-full focus:outline-none focus:shadow-outline mt-4 block"
            >
              Delete Job
            </button>
          </div>
        </aside>
      </div>
    </div>
  </section>
</template>

<style>
/* HTML: <div class="loader"></div> */
.loader-parent {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 300px; /* Adjust as needed */
}

.loader {
  --d: 22px;
  width: 4px;
  height: 4px;
  border-radius: 50%;
  color: #084b09;
  box-shadow:
    calc(1 * var(--d)) calc(0 * var(--d)) 0 0,
    calc(0.707 * var(--d)) calc(0.707 * var(--d)) 0 1px,
    calc(0 * var(--d)) calc(1 * var(--d)) 0 2px,
    calc(-0.707 * var(--d)) calc(0.707 * var(--d)) 0 3px,
    calc(-1 * var(--d)) calc(0 * var(--d)) 0 4px,
    calc(-0.707 * var(--d)) calc(-0.707 * var(--d)) 0 5px,
    calc(0 * var(--d)) calc(-1 * var(--d)) 0 6px;
  animation: l27 1s infinite steps(8);
}
@keyframes l27 {
  100% {
    transform: rotate(1turn);
  }
}
</style>
