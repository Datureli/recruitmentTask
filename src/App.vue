<template>
  <div class="formDiv">
    <form @submit.prevent="submitForm">
      <div class="form-group">
        <p>
          {{ form.descriptionMaxLength - form.description.length }} /
          {{ form.descriptionMaxLength }}
        </p>
        <label for="Description">Description</label>

        <textarea v-model="form.description" maxlength="255"> </textarea>

        <p class="errors" v-if="!descriptionValidation">Text is required</p>
        <p class="errors" v-if="!descriptionLengthValidation">
          You can’t enter more than 255 characters
        </p>
      </div>
      <div class="form-group">
        <input type="radio" value="yes" v-model="form.sendInformation" />
        <label for="Send confirmation">Yes</label>
        <input type="radio" value="no" v-model="form.sendInformation" />
        <label for="Send confirmation">No</label>
        <p v-if="radiusButton" class="errors">Text is required</p>
      </div>

      <select @change="changeDisable" v-model="vatInput">
        <option disabled hidden>Choose vat</option>
        <option :value="19">19%</option>
        <option :value="21">21%</option>
        <option :value="23">23%</option>
        <option :value="25">25%</option>
      </select>
      <label for="price Netto EUR">price Netto EUR</label>
      <input type="text" :disabled="isDisabled" />
      <label for="price Brutto EUR">price Brutto EUR</label>
      <input type="text" :value="vatInput" :disabled="isDisabled" />
      <button type="submit">Submit</button>
    </form>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      disabled: false,
      form: {
        description: "",
        vatInput: "",
        sendInformation: null,
        radiusButton: null,
        bruttoPriceInput: null,
        nettoPriceInput: null,
        descriptionMaxLength: 255,
      },
    };
  },
  methods: {
    submitForm() {
      const radioButtonValidation = !!this.radiusButton;
      const formValidation =
        this.descriptionValidation &&
        this.descriptionLengthValidation &&
        radioButtonValidation &&
        this.disableBrutto;
      if (formValidation) {
        return "good";
      } else {
        return "wrong";
      }
    },
    changeDisable() {
      this.disabled = true
    },
  },
  computed: {
    isDisabled() {
     return this.disabled === false
    },

    descriptionValidation() {
      return !!this.form.description;
    },
    descriptionLengthValidation() {
      return this.form.description.length < 255;
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.formDiv {
  text-align: center;
  margin: auto;
  display: grid;
}
div {
  text-align: center;
}
form {
  width: 400px;
  height: auto;
  margin: auto;
  position: relative;
  display: grid;
  justify-content: center;
  text-align: center;
  border: 4px solid black;
}
.form-group {
  display: grid;
}
textarea {
  align-items: center;
  width: 300px;
  height: 50px;
}
input {
}
.errors {
  color: red;
}
</style>
