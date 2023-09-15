<script setup>
const people = ref([]);
const selected = ref([]);

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
    direction: "desc",
  },
  {
    key: "role",
    label: "Role",
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

const getData = async () => {
  try {
    const response = await fetch("http://localhost:9999/people");

    if (!response.ok) {
      throw new Error(`Error! status: ${response.status}`);
    }

    const data = await response.json();
    people.value = data;
  } catch (error) {
    console.log(error);
  }
};

console.log("test");

onMounted(() => {
  getData();
});

const toAnotherPage = (row) => {
  navigateTo("/abc/" + row);
};

const deleteRow = (row) => {
  console.log("deleted row ", row);
};
</script>

<template>
  <div class="max-w-7xl mx-auto p-8">
    <UTable :rows="people" :columns="columns">
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
