<template>
  <div class="formDiv">
    <form @submit.prevent="submitForm">
      <h1>Calculating Form</h1>
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

      <div class="form-group">
        <select @change="changeDisable" :value="form.vatInput" v-model="form.vatInput">
          <option :value="form.vatInput" disabled hidden>Choose vat</option>
          <option v-for="option in options" :key="option.value" :value="option.value">
            {{ option.value + "%" }}
          </option>
        </select>
        <label for="price Netto EUR" >price Netto EUR</label>
        <input
          type="number"
          v-model="form.nettoPriceInput"
          :disabled="isDisabled"
        />
        <p class="errors" v-if="!nettoInput"></p>
        <label for="price Brutto EUR">price Brutto EUR</label>
        <input type="number" :value="vatInput" disabled />
      </div>
      <button type="submit">Submit</button>
      {{ calculateBrutto }}
      {{form.vatInput}}
    </form>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      options: [
        {
          value: 19,
        },
        {
          value: 21,
        },
        {
          value: 23,
        },
        {
          value: 25,
        },
      ],
      disabled: false,
      form: {
        description: "",
        vatInput: 0,
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
        this.nettoInput &&
        this.disableBrutto;
      if (formValidation) {
        return "good";
      } else {
        return "wrong";
      }
    },
    changeDisable() {
      this.disabled = true;
    },
  },
  computed: {
    isDisabled() {
      return this.disabled === false;
    },
    calculateBrutto() {
      
      let calculateVat = (this.form.nettoPriceInput / 100) * this.form.vatInput
      return this.form.nettoPriceInput - calculateVat;
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
