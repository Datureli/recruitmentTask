<template>
  <div class="formDiv">
    <form @submit.prevent="submitForm">
      <h1>Calculating Form</h1>
      <div class="form-group">
        <div class="radioButton">
          <label for="Description">Description</label>
          <p>
            {{ form.descriptionMaxLength - form.description.length }} /
            {{ form.descriptionMaxLength }}
          </p>
        </div>

        <textarea v-model="form.description" maxlength="255"  > </textarea>

        <p class="errors " v-if="!descriptionValidation">Text is required</p>
        <p class="errors" v-if="!descriptionLengthValidation">
          You can’t enter more than 255 characters
        </p>
      </div>

      <div class="form-group">
        <select
          style="margin-bottom: 20px"
          @change="changeDisable"
          :value="form.vatInput"
          v-model="form.vatInput"
        >
          <option :value="form.vatInput" disabled hidden>Choose vat</option>
          <option
            v-for="option in options"
            :key="option.value"
            :value="option.value"
          >
            {{ option.value + "%" }}
          </option>
        </select>
                <p class="errors" v-if="!chooseVatValidation">Text is required</p>
        <label for="price Netto EUR">Price Netto EUR</label>
        <input
          type="number"
          v-model="form.nettoPriceInput"
          :disabled="isDisabled"
        />
        <p class="errors" v-if="!form.nettoPriceInput">Text is required</p>

        <label for="price Brutto EUR">Price Brutto EUR</label>
        <input type="number" :value="calculateBrutto" disabled />
        
      </div>
      <div class="form-group">
        <label for="Send confirmation">Send Confirmation</label>
        <div class="displayFlex">
          <label for="yes" >Yes</label>
          <input type="radio" name="yesOrNo" v-model="form.radioButton" />
        </div>
        <div class="displayFlex">
          <label for="no"  >No</label>
          <input type="radio" name="yesOrNo" v-model="form.radioButton" />
        </div>
        <p v-if="!validateRadioButton" class="errors">Text is required</p>
      </div>
      <button type="submit" :disabled="!validateForm">Submit</button>
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
        radioButton: null,
        bruttoPriceInput: null,
        nettoPriceInput: null,
        descriptionMaxLength: 255,
      },
    };
  },
  methods: {
    submitForm() {
      if (this.validateForm) {
        return "good";
      } else {
        return "wrong";
      }
    },
    changeDisable() {
      this.disabled = true;
    },
    radioButtonValidation() {
      return !!this.form.radioButton;
    },
  },
  computed: {
    isDisabled() {
      return this.disabled === false;
    },
    calculateBrutto() {
      let calculateVat = (this.form.nettoPriceInput / 100) * this.form.vatInput;
      return this.form.nettoPriceInput - calculateVat;
    },
    validateForm() {
      return (
        this.descriptionValidation &&
        this.descriptionLengthValidation &&
        this.nettoPriceValidation &&
        this.chooseVatValidation &&
        this.validateRadioButton
      );
    },
    validateRadioButton() {
      return !!this.form.radioButton
    },
    chooseVatValidation() {
      return !!this.form.vatInput
    },
    nettoPriceValidation() {
      return !!this.form.nettoPriceInput
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
  justify-content: center;
  color: #2c3e50;
  margin-top: 20px;
}
h1 {
  margin: 0;
}
form {
  width: 400px;
  height: auto;
  margin: auto;
  padding: 20px 30px;
  display: grid;
  justify-content: center;
  text-align: center;
  box-shadow: -2px 0px 24px -9px rgba(66, 68, 90, 1);
}
.form-group {
  display: grid;
  margin-bottom: 10px;
  padding-bottom: 20px;
  position: relative;
}
.radioButton {
  display: flex;
}
.errors {
  color: red;
}
label {
  display: inline-block;
  font-size: 15px;
  font-weight: 800;
}

input,
textarea,
option {
  border: 2px solid #b9acac;
  border-radius: 4px;
  text-align: center;
  display: block;
  font-size: 14px;
  padding: 10px;
}
textarea {
  text-align: left;
}
option {
  text-align: left;
}
button {
  border-radius: 6px;
  display: block;
  font-size: 20px;
  padding: 10px;
  cursor: pointer;
  transition: 0.8s;
}
p {
  margin: auto;
  font-size: 15px;
}
.displayFlex {
  display: flex;
  text-align: center;
  justify-content: center;
}
input:focus {
	outline: 0;
	border-color: #777;
}
</style>
