<script setup>
// const people = ref([]);

const columns = [
  {
    key: "id",
    label: "ID",
  },
  {
    key: "name",
    label: "Name",
    sortable: true,
  },
  {
    key: "title",
    label: "Title",
    sortable: true,
  },
  {
    key: "email",
    label: "Email",
    sortable: true,
  },
  {
    key: "role",
    label: "Role",
    direction: "desc",
  },
  {
    key: "actions",
    label: "Action",
  },
];

const items = (row) => [
  [
    {
      label: "Edit",
      icon: "i-heroicons-pencil-square-20-solid",
      click: () => toAnotherPage(row.id),
    },
    {
      label: "Delete",
      icon: "i-heroicons-trash-20-solid",
      click: () => deleteRow(row.id),
    },
  ],
];

// const getData = async () => {
//   try {
//     const response = await fetch("http://localhost:9999/people");

//     if (!response.ok) {
//       throw new Error(`Error! status: ${response.status}`);
//     }

//     const data = await response.json();
//     people.value = data;
//   } catch (error) {
//     console.log(error);
//   }
// };

// onMounted(() => {
//   getData();
// });

const { pending, data: people } = await useLazyAsyncData("people", () =>
  $fetch("http://localhost:9999/people")
);

const toAnotherPage = (row) => {
  navigateTo("/abc/" + row);
};

const deleteRow = async (id) => {
  try {
    const result = confirm("apakah anda yakin?");

    if (result) {
      const response = await fetch("http://localhost:9999/people/" + id, {
        method: "DELETE",
      });
      people.value = people.value.filter((data) => data.id !== id);
    }
  } catch (error) {
    console.log(error);
  }
};
</script>

<template>
  <div class="max-w-7xl mx-auto p-8">
    <div class="flex justify-end mb-6">
      <UButton size="lg" to="/abc/create"> Create Data</UButton>
    </div>

    <UTable
      :rows="people"
      :columns="columns"
      :loading="pending"
      :loading-state="{
        icon: 'i-heroicons-arrow-path-20-solid',
        label: 'Loading...',
      }"
    >
      <template #actions-data="{ row }">
        <UDropdown :items="items(row)">
          <UButton
            color="gray"
            variant="ghost"
            icon="i-heroicons-ellipsis-horizontal-20-solid"
          />
        </UDropdown>
      </template>
    </UTable>
  </div>
</template>
