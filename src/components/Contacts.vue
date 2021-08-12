<template>
  <div>
    <!-- MAIN HEADER -->
    <h1>{{ title }}</h1>

    <!-- INPUT DATA -->
    <div v-if="isActivePage">
      <div v-if="!isAlertDelete">
        <input
          class="inputValue"
          v-model="inputValue"
          type="text"
          placeholder="Enter the name"
          ref="fullname"
          @keypress.enter="addContact"
        />
        <Button class="btn primary" text="Add Contact" @click="addContact" />

        <!-- WORK WITH CONTACTS -->
        <div v-if="contacts.length === 0">
          <hr />
          <h3>No contacts. Add someone</h3>
        </div>
        <div :key="contact" v-for="(contact, i) in contacts">
          <li class="list-item" @click="moveToInfo(i)">
            <span class="bold"> {{ contact.fullName }}</span>
            <Button
              class="btn danger"
              text="Delete"
              @click.stop
              @click="alertDelete(i, contact)"
            />
          </li>
        </div>
      </div>

      <!-- ALERT DELETE THE CONTACT -->
      <div v-if="isAlertDelete">
        <h3>Delete "{{ deletedContactName }}" ?</h3>
        <Button class="btn primary" text="Yes" @click="deleteContact" />
        <Button
          class="btn danger"
          text="No"
          @click="isAlertDelete = !isAlertDelete"
        />
      </div>
    </div>

    <!-- SWITCH TO THE INFORMATION / CONTACT LIST -->
    <div v-if="!isActivePage">
      <Info :extraInfo="extraInfo" />
      <hr />
      <Button class="btn extra" text="Contact List" @click="moveBack" />
    </div>
  </div>
</template>

<script>
import Button from "./Button";
import Info from "./Info";

export default {
  name: "Contacts",
  props: {
    contacts: Array,
    extraInfo: Array,
  },
  components: {
    Button,
    Info,
  },

  data() {
    return {
      title: "Contact List",
      inputValue: "",
      isActivePage: true,
      isAlertDelete: false,
      deletedContactID: "",
      deletedContactName: "",
    };
  },
  methods: {
    addContact() {
      if (this.inputValue !== "") {
        const newName = this.$refs.fullname.value;
        this.$emit("add-contact", newName);
      }
      this.inputValue = "";
    },
    alertDelete(id, contactName) {
      this.isAlertDelete = !this.isAlertDelete;
      this.deletedContactID = id;
      this.deletedContactName = contactName.fullName;
    },
    deleteContact() {
      this.$emit("del-contact", this.deletedContactID);
      this.isAlertDelete = !this.isAlertDelete;
    },
    moveToInfo(id) {
      this.title = "Contact information";
      this.isActivePage = !this.isActivePage;
      this.$emit("move-info", id);
    },
    moveBack() {
      this.isActivePage = !this.isActivePage;
      this.title = "Contact List";
    },
  },
};
</script>
