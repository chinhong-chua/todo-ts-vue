<script setup lang="ts">
import { defineComponent, onMounted, reactive, ref } from "vue";
import TutorialDataService from "@/services/TutorialDataService";
import Tutorial from "@/types/Tutorial";
import ResponseData from "@/types/ResponseData";
import { useRoute, useRouter } from "vue-router";

const router = useRouter();

const route = useRoute();

const currentTutorial: Tutorial = reactive({
  id: null,
  title: "",
  description: "",
  published: false,
});

const message = ref("" as String);

const getTutorial = (id: any) => {
  TutorialDataService.get(id)
    .then((response: ResponseData) => {
      Object.assign(currentTutorial, response.data);
      // currentTutorial = response.data;
      console.log(response.data);
    })
    .catch((e: Error) => {
      console.log(e);
    });
};

const updatePublished = (status: boolean) => {
  let data = {
    id: currentTutorial.id,
    title: currentTutorial.title,
    description: currentTutorial.description,
    published: status,
  };

  TutorialDataService.update(currentTutorial.id, data)
    .then((response: ResponseData) => {
      console.log(response.data);
      currentTutorial.published = status;
      message.value = "The status was updated successfully!";
    })
    .catch((e: Error) => {
      console.log(e);
    });
};

const updateTutorial = () => {
  TutorialDataService.update(currentTutorial.id, currentTutorial)
    .then((response: ResponseData) => {
      console.log(response.data);
      message.value = "The tutorial was updated successfully!";
    })
    .catch((e: Error) => {
      console.log(e);
    });
};

const deleteTutorial = () => {
  TutorialDataService.delete(currentTutorial.id)
    .then((response: ResponseData) => {
      console.log(response.data);
      router.push({ name: "tutorials" });
    })
    .catch((e: Error) => {
      console.log(e);
    });
};

onMounted(() => {
  {
    message.value = "";
    getTutorial(route.params.id);
  }
});
</script>

<template>
  <div v-if="currentTutorial.id" class="edit-form">
    <h4>Tutorial</h4>
    <form class="mb-3">
      <div class="form-group">
        <label for="title">Title</label>
        <input
          type="text"
          class="form-control"
          id="title"
          v-model="currentTutorial.title"
        />
      </div>
      <div class="form-group">
        <label for="description">Description</label>
        <input
          type="text"
          class="form-control"
          id="description"
          v-model="currentTutorial.description"
        />
      </div>

      <div class="form-group">
        <label><strong>Status:</strong></label>
        {{ currentTutorial.published ? "Published" : "Pending" }}
      </div>
    </form>

    <button
      class="btn btn-primary ma-3"
      v-if="currentTutorial.published"
      @click="updatePublished(false)"
    >
      UnPublish
    </button>
    <button v-else class="btn btn-secondary" @click="updatePublished(true)">
      Publish
    </button>

    <button class="btn btn-danger me-3 ms-3" @click="deleteTutorial">
      Delete
    </button>

    <button type="submit" class="btn btn-success" @click="updateTutorial">
      Update
    </button>
    <p>{{ message }}</p>
  </div>

  <div v-else>
    <br />
    <p>Please click on a Tutorial...</p>
  </div>
</template>

<style>
.edit-form {
  max-width: 300px;
  margin: auto;
}
</style>

function useRouter() { throw new Error("Function not implemented."); } function
useRouter() { throw new Error("Function not implemented."); }
