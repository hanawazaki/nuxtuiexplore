<template>
  <UContainer :ui="ui">
    <UCard shadow>
      <template #header>
        <div class="flex justify-between">
          <h3
            class="text-base font-semibold leading-6 text-gray-900 dark:text-white"
          >
            Create Data
          </h3>

          <UButton
            icon="i-heroicons-arrow-left-20-solid"
            @click="() => navigateTo('/')"
            >Back
          </UButton>
        </div>
      </template>

      <div>
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

          <UButton size="lg" type="submit"> Save </UButton>
        </UForm>
      </div>
    </UCard>
  </UContainer>
</template>

<script setup>
import { object, string } from "yup";

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
  name: string().required("Nama tidak boleh kosong"),
});

const submitData = async (submit) => {
  try {
    const response = await fetch("http://localhost:9999/people/", {
      method: "POST",
      headers: {
        "Content-type": "application/json",
      },
      body: JSON.stringify(submit.data),
    });

    const data = response.json();
    state.value = data;

    navigateTo("/");
  } catch (error) {
    console.log(error);
  }
};
</script>

<style lang="scss" scoped></style>
