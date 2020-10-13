<template>
  <div id="app">
      <h1> Address Book </h1>
      <div id ="form-div">
          <contacts-details-form @add:contact="createContact" />
      </div>
      <contacts
        :contacts="contacts"
        @delete:contact="removeContact"
        @edit:contact="editContact"
      />
  </div>
</template>

<script>
import Contacts from './components/Contacts';
import ContactsDetailsForm from './components/ContactDetailsForm';
import axios from "axios";
let apiUrl = "https://540f3e3e99.execute-api.us-east-1.amazonaws.com/dev/api/contacts";

export default {
  name: 'App',
  components: {
    Contacts,
    ContactsDetailsForm,
  },
  data() {
    return {
      contacts: []
    }  
  },
  created() {
    axios
      .get(apiUrl)
      .then((response) => {
        // console.log(response.data);
        this.contacts = response.data.data;
      })
      .catch((e) => {
        console.log(e);
      });
  },
  methods: {
    createContact(contact) {
      let newContact;
      axios.post(apiUrl, {...contact})
      .then((res) => {
        newContact = res.data.data;
        this.contacts = [...this.contacts, newContact];
      })
      .catch((err) => {
        console.log(err);
      })
    },
    removeContact(contactID) {
      this.contacts = this.contacts.filter(
        (contact) => contact._id !== contactID
      );
      const contactRemoved = this.contacts.filter(
        (contact) => contact._id === contactID
      )[0];
      axios.delete(apiUrl + "/" + contactID)
      .catch((err) => {
        console.log(err);
        this.contacts = [...this.contacts, contactRemoved];
      });
    },
    editContact(contactID, updatedContact) {
      const prevContact = this.contacts.filter(
        (contact) => contact._id === contactID
      )[0];
      axios.put(apiUrl, "/" + contactID, {...updatedContact})
        .catch((err) => {
        console.log(err);
        this.contacts = this.contacts.map((contact) =>
        contact._id === contactID ? prevContact : contact);
      });
    }
  }
}
</script>

<style>
  * {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
  }
  body {
    font-family: Arial, Helvetica, sans-serif;
    line-height: 1.4;
  }
  #app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  }

  #form-div {
    margin: 2em 25%;
    text-align: left;
  }
  
</style>
