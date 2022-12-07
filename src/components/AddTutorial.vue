<script setup lang="ts">
import TutorialDataService from "../services/TutorialDataService";
import Tutorial from "../types/Tutorial";
import ResponseData from "../types/ResponseData";
import { reactive, ref } from "@vue/reactivity";

const initTutorial = {
  id: null,
  title: "",
  description: "",
  published: false,
};
const tutorial = reactive(initTutorial as Tutorial);

const submitted = ref(false);

const saveTutorial = () => {
  let data = {
    title: tutorial.title,
    description: tutorial.description,
  };

  TutorialDataService.create(data)
    .then((response: ResponseData) => {
      tutorial.id = response.data.id;
      console.log(response.data);
      submitted.value = true;
    })
    .catch((e: Error) => {
      console.log(e);
    });
};

const newTutorial = () => {
  submitted.value = false;
  Object.assign(tutorial, initTutorial) as Tutorial;
};
</script>

<template>
  <div class="submit-form">
    <div v-if="!submitted">
      <div class="form-group">
        <label for="title">Title</label>
        <input
          type="text"
          class="form-control"
          id="title"
          required
          v-model="tutorial.title"
          name="title"
        />
      </div>

      <div class="form-group">
        <label for="description">Description</label>
        <input
          class="form-control"
          id="description"
          required
          v-model="tutorial.description"
          name="description"
        />
      </div>

      <button @click="saveTutorial" class="btn btn-success mt-3">Submit</button>
    </div>

    <div v-else>
      <h4>You submitted successfully!</h4>
      <button class="btn btn-success" @click="newTutorial">Add</button>
    </div>
  </div>
</template>
<style>
.submit-form {
  max-width: 300px;
  margin: auto;
}
</style>