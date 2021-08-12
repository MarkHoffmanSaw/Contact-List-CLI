<template>
  <div>
    <!-- EDITING HEADER -->
    <h2 v-if="isEditing && !isAlertEdit">Editing:</h2>

    <!-- INPUT DATA -->
    <div v-if="!isAlertDelete && !isAlertEdit">
      <input
        class="inputValue"
        v-model="inputExtraName"
        type="text"
        placeholder="info 1"
        @keypress.enter="addInfo"
      />
      <input
        class="inputValue"
        v-model="inputExtraInfo"
        type="text"
        placeholder="info 2"
        @keypress.enter="addInfo"
      />

      <!-- BUTTONS FOR INPUT DATA (CHANGING) -->
      <span v-if="!isEditing">
        <Button class="btn primary" text="Add Info" @click="addInfo" />
        <Button class="btn extra" text="Step back" @click="stepBack" />
      </span>
      <span v-if="isEditing">
        <Button class="btn primary" text="Save info" @click="saveInfo" />
        <Button class="btn danger" text="Cancel edit" @click="alertEdit" />
      </span>

      <!-- WORK WITH INFORMATION -->
      <div v-if="extraInfo.length === 0">
        <hr />
        <h3>No information about the contact</h3>
      </div>
      <div v-if="!isEditing" :key="info" v-for="(info, i) in extraInfo">
        <li class="list-item">
          <span class="bold"> {{ info }}</span>
          <Button class="btn warning" text="Edit" @click="editInfo(i)" />
          <Button
            class="btn danger"
            text="Delete the info"
            @click="alertDelete(i)"
          />
        </li>
      </div>
    </div>

    <!-- ALERT DELETE THE INFO -->
    <div v-if="isAlertDelete">
      <h3>Delete the info?</h3>
      <Button class="btn primary" text="Yes" @click="deleteInfo" />
      <Button
        class="btn danger"
        text="No"
        @click="isAlertDelete = !isAlertDelete"
      />
    </div>

    <!-- ALERT STOP EDITING -->
    <div v-if="isAlertEdit">
      <h3>Stop editing?</h3>
      <h4>(Data will not be saved)</h4>
      <Button
        class="btn primary"
        text="Yes"
        @click="
          (isAlertEdit = false),
            (isEditing = false),
            (inputExtraName = ''),
            (inputExtraInfo = '')
        "
      />
      <Button
        class="btn danger"
        text="No"
        @click="isAlertEdit = !isAlertEdit"
      />
    </div>
  </div>
</template>

<script>
import Button from "./Button";

export default {
  name: "Info",
  components: {
    Button,
  },
  props: {
    extraInfo: Array,
  },
  data() {
    return {
      inputExtraName: "",
      inputExtraInfo: "",
      isActivePage: true,
      isAlertDelete: false,
      isEditing: false,
      isAlertEdit: false,
      deletedInfo: "",
      currentEdit: "",
      thisIndexArr: "",
      editExtraInfoArr: false,
      exitExtraInfoID: false,
    };
  },
  methods: {
    addInfo() {
      if (this.inputExtraName && this.inputExtraInfo !== "") {
        this.extraInfo.push(`${this.inputExtraName}:${this.inputExtraInfo}`);
        this.editExtraInfoArr = true;
      }
      this.inputExtraName = "";
      this.inputExtraInfo = "";
    },
    alertDelete(id) {
      this.isAlertDelete = !this.isAlertDelete;
      this.deletedInfo = id;
    },
    deleteInfo() {
      this.extraInfo.splice(this.deletedInfo, 1);
      this.isAlertDelete = !this.isAlertDelete;
    },
    moveEdit(i) {
      this.isActivePage = !this.isActivePage;
    },
    editInfo(i) {
      this.isEditing = !this.isEditing;
      const splitedArr = this.extraInfo[i].split(":");
      this.inputExtraName = splitedArr[0];
      this.inputExtraInfo = splitedArr[1];
      this.currentEdit = this.extraInfo[i];
    },
    saveInfo() {
      this.indexArr = this.extraInfo.indexOf(this.currentEdit);
      this.extraInfo.splice(
        this.indexArr,
        1,
        `${this.inputExtraName}:${this.inputExtraInfo}`
      );
      this.isEditing = !this.isEditing;
      this.inputExtraName = "";
      this.inputExtraInfo = "";
      this.editExtraInfoID = true;
    },
    alertEdit() {
      this.isAlertEdit = !this.isAlertEdit;
      // The rest of the operations of editing are performed in the <BUTTON /> component
    },
    stepBack() {
      // for an element of the Array "extraInfo[id]"
      if (this.editExtraInfoID) {
        this.extraInfo.splice(this.indexArr, 1, this.currentEdit);
        this.editExtraInfoID = false;
      }
      // for an Array "extraInfo"
      else if (this.editExtraInfoArr) {
        this.extraInfo.pop();
        this.editExtraInfoArr = false;
      }
    },
  },
};
</script>
