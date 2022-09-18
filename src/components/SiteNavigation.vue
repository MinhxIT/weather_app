<template>
  <header class="sticky bg-weather-primary top-0 shadow-lg">
    <nav
      class="
        container
        flex flex-col
        sm:flex-row
        items-center
        gap-4
        text-white
        py-6
      "
    >
      <RouterLink :to="{ name: 'home' }">
        <div class="flex items-center gap-3">
          <i class="fa-solid fa-sun text-2xl"></i>
          <p class="text-2xl">The local weather</p>
        </div>
      </RouterLink>
      <div class="flex flex-1 gap-3 justify-end">
        <i
          class="
            fa-solid fa-circle-info
            text-xl
            hover:text-weather-secondary
            cursor-pointer
            duration-200
          "
          @click="toggleModal"
        ></i>
        <i
          class="
            fa-solid fa-plus
            text-xl
            hover:text-weather-secondary
            cursor-pointer
            duration-200
          "
          @click="addCity"
          v-if="route.query.preview"
        ></i>
      </div>
      <BaseModal :modalActive="modalActive" @close-modal="toggleModal">
        <div class="text-black">
          <h1 class="text-2xl mb-1">About:</h1>
          <p class="mb-4">
            The Local Weather allows you to track the current and future weather
            of cities of your choosing.
          </p>
          <h2 class="text-2xl">How it works:</h2>
          <ol class="list-decimal list-inside mb-4">
            <li>
              Search for your city by entering the name into the search bar.
            </li>
            <li>
              Select a city within the results, this will take you to the
              current weather for your selection.
            </li>
            <li>
              Track the city by clicking on the "+" icon in the top right. This
              will save the city to view at a later time on the home page.
            </li>
          </ol>

          <h2 class="text-2xl">Removing a city</h2>
          <p>
            If you no longer wish to track a city, simply select the city within
            the home page. At the bottom of the page, there will be am option to
            delete the city.
          </p>
        </div>
      </BaseModal>
    </nav>
  </header>
</template>

<script setup>
import { ref } from "vue";
import { RouterLink, useRoute, useRouter } from "vue-router";
import BaseModal from "./BaseModal.vue";
import { uid } from "uid";

const savedCities = ref([]);
const modalActive = ref(null);
const route = useRoute();
const router = useRouter();

const addCity = () => {
  if (localStorage.getItem("savedCities")) {
    savedCities.value = JSON.parse(localStorage.getItem("savedCities"));
  }
  const locationObj = {
    id: uid(),
    state: route.params.state,
    city: route.params.city,
    coords: {
      lat: route.query.lat,
      lng: route.query.lng,
    },
  };
  savedCities.value.push(locationObj);
  localStorage.setItem("savedCities", JSON.stringify(savedCities.value));

  let query = Object.assign({}, route.query);
  delete query.preview;
  query.id = locationObj.id;
  router.replace({ query });
};

const toggleModal = () => {
  modalActive.value = !modalActive.value;
};
</script>
