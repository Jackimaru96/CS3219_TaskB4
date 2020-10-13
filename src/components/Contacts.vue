<template>
    <div id="contacts"> 
        <table>
      <thead>
        <tr>
          <th>Name</th>
          <th>Gender</th>
          <th>Phone Number</th>
          <th>Email</th>
          <th>Edit/Delete Contact</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="contact in contacts" :key="contact._id">
          <!-- Name -->
          <td v-if="editID === contact._id">
            <input type="text" v-model="contact.name" />
          </td>
          <td v-else>{{ contact.name }}</td>
          <!-- Gender -->
          <td v-if="editID === contact._id">
            <input type="text" v-model="contact.gender" />
          </td>
          <td v-else>{{ contact.gender }}</td>
          <!-- Phone Number -->
          <td v-if="editID === contact._id">
            <input type="text" v-model="contact.phone" />
          </td>
          <td v-else>{{ contact.phone }}</td>
          <!-- Email -->
          <td v-if="editID === contact._id">
          <input type="text" v-model="contact.email" />
          </td>
          <td v-else>{{ contact.email }}</td>
          <!-- If in edit mode, show save and cancel buttons -->
          <td v-if="editID === contact._id">
            <b-button @click="editContact(contact)" variant="success">Save</b-button>
            <b-button class="muted-button" @click="editID = null">Cancel</b-button>
          </td>
          <!-- else show edit and delete buttons -->
          <td v-else>
            <b-button @click="editMode(contact._id)" variant="warning">Edit</b-button>
            <b-button variant="danger" @click="handleDelete(contact._id)"> Delete </b-button>
          </td>
        </tr>
      </tbody>
    </table>

    </div>

</template>

<script>
export default {
  name: "contacts-table",
  data() {
    return {
      editID: null,
    };
  },
  props: {
    contacts: Array,
  },
  methods: {
    handleDelete(id) {
      this.$emit("delete:contact", id);
    },
    editMode(id) {
      this.editID = id;
    },
    editContact(contact) {
      if (
        contact.name === "" ||
        contact.email === "" ||
        contact.gender === "" ||
        contact.phone === ""
      )
        return;
      this.$emit("edit:contact", contact._id, contact);
      this.editID = null;
    },
  },
};
</script>

<style scoped>
div table {
  width: 100%;
  border-collapse: separate;
}
div th, td {
  padding: 15px;
  text-align: center;
}
div th {
  background-color: #83ccee;
  color: white;
  font-weight: 400;
}
div tr:nth-child(even) {
  background-color: #f2f2f2;
}
button {
  margin-left: 12px;
}
</style>
