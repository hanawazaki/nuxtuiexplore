<template>
  <UContainer :ui="ui">
    <UCard shadow>
      <template #header>
        <div class="flex justify-between">
          <h3
            class="text-base font-semibold leading-6 text-gray-900 dark:text-white"
          >
            Edit Data
          </h3>

          <UButton
            icon="i-heroicons-arrow-left-20-solid"
            @click="() => navigateTo('/')"
            >Back
          </UButton>
        </div>
      </template>

      <div v-show="dataloaded">
        <UForm :schema="schema" :state="state" @submit="submitData">
          <UFormGroup size="lg" label="Name" name="name" class="mb-6">
            <UInput
              placeholder=""
              icon="i-heroicons-user"
              v-model="state.name"
            />
          </UFormGroup>

          <UFormGroup size="lg" label="Title" name="title" class="mb-6">
            <UInput
              placeholder=""
              icon="i-heroicons-briefcase"
              v-model="state.title"
            />
          </UFormGroup>

          <UFormGroup size="lg" label="Email" name="email" class="mb-6">
            <UInput
              placeholder=""
              icon="i-heroicons-envelope"
              v-model="state.email"
            />
          </UFormGroup>

          <UFormGroup size="lg" label="Role" name="role" class="mb-6">
            <USelectMenu
              icon="i-heroicons-user-group"
              v-model="state.role"
              :options="roles"
              size="lg"
            />
          </UFormGroup>

          <UButton size="lg" type="submit"> Update </UButton>
        </UForm>
      </div>

      <div v-show="!dataloaded">
        <h1>LOADING</h1>
      </div>
    </UCard>
  </UContainer>
</template>

<script setup>
import { object, string } from "yup";

const { id } = useRoute().params;
const dataloaded = ref(false);
const ui = {
  base: "mx-auto",
  padding: "px-4 sm:px-6 lg:px-8 mt-8",
  constrained: "max-w-3xl",
};
const roles = ref(["Admin", "Member"]);

const state = ref({
  name: null,
  title: null,
  email: null,
  role: null,
});

const schema = object({
  email: string().email("Invalid email").required("Required"),
  password: string()
    .min(8, "Must be at least 8 characters")
    .required("Required"),
  name: string().required("Nama tidak boleh kosong"),
});

async function getData() {
  try {
    const response = await fetch("http://localhost:9999/people/" + id);

    if (!response.ok) {
      throw new Error(`Error! status: ${response.status}`);
    }

    const data = await response.json();
    state.value = data;

    dataloaded.value = true;

    console.log("get data", state.value);
  } catch (error) {
    console.log(error);
  }
}

const submitData = (submit) => {
  console.log(submit.data);
};

onMounted(() => {
  getData();
});
</script>

<style lang="scss" scoped></style>
