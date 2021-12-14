<template>
  <div>
    <form v-if="!successStatus" @submit.prevent="veeSubmitForm">
      <h1>Calculating Form</h1>
      <div class="form-group">
        <p>
          {{ form.descriptionMaxLength - form.description.length }} /
          {{ form.descriptionMaxLength }}
        </p>

        <label for="Description">Description </label>

        <textarea
          v-model="form.description"
          @blur="v$.form.description.$touch"
          maxlength="255"
        >
        </textarea>
        <div class="errors" v-if="v$.form.description.$error">
          Text is required
        </div>

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
          @blur="v$.form.vatInput.$touch"
          name="myselect"
        >
          <option value disabled>Choose vat</option>
          <option
            v-for="option in options"
            :key="option.value"
            :value="option.value"
          >
            {{ option.value + "%" }}
          </option>
        </select>

        <div class="errors" v-if="v$.form.vatInput.$error">
          Text is required
        </div>
        <label for="price Netto EUR">Price Netto EUR</label>
        <p class="errors" v-if="!isNumber && form.nettoPriceInput.length > 0">
          Please,input number
        </p>
        <input
          type="number"
          v-model.number="form.nettoPriceInput"
          :disabled="isDisabled"
          @blur="v$.form.vatInput.$touch"
        />
        <div class="errors" v-if="v$.form.nettoPriceInput.$error">
          Text is required
        </div>

        <label for="price Brutto EUR">Price Brutto EUR</label>
        <input type="number" :value="calculateBrutto" disabled />
      </div>
      <div class="form-group">
        <label for="Send confirmation">Send Confirmation</label>
        <div>
          <label for="yes"
            >Yes
            <input
              type="radio"
              name="yesOrNo"
              v-model="form.radioButton"
              @blur="v$.form.vatInput.$touch"
            />
          </label>

          <label for="no"
            >No
            <input
              type="radio"
              name="yesOrNo"
              v-model="form.radioButton"
              @blur="v$.form.vatInput.$touch"
            />
          </label>
        </div>
      </div>
      <div class="errors" v-if="v$.form.radioButton.$error">
        Text is required
      </div>

      <button type="submit">Submit</button>
    </form>
    <div class="successStatus" v-else>
      <p class="successStatusParagraph">Congratulations!</p>
    </div>
  </div>
</template>

<script>
import useVuelidate from "@vuelidate/core";
import { required, alphaNum } from "@vuelidate/validators";

export default {
  name: "App",
  setup() {
    return {
      v$: useVuelidate(),
    };
  },
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
      successStatus: false,
      form: {
        description: "",
        vatInput: "",
        radioButton: null,
        bruttoPriceInput: null,
        nettoPriceInput: "",
        descriptionMaxLength: 255,
      },
    };
  },
  validations() {
    return {
      form: {
        description: {
          required,
          $autoDirty: true,
        },
        vatInput: {
          required,

          $autoDirty: true,
          $lazy: true,
        },
        nettoPriceInput: {
          required,
          alphaNum,
        },
        radioButton: {
          required,
        },
      },
    };
  },
  methods: {
    async veeSubmitForm() {
      const isFormCorrect = await this.v$.$validate();
      if (!isFormCorrect) {
        return;
      } else {
        this.successStatus = true;
      }
    },
    changeDisable() {
      this.disabled = true;
    },
    showSuccessStatus() {
      this.successStatus = true;
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
      return this.descriptionLengthValidation && this.isNumber;
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
  height: 500px;
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
  position: relative;
}
.radioButton {
  display: flex;
}
.errors {
  display: block;
  text-align: center;
  color: red;
  font-weight: 700;
}
label {
  display: inline-block;
  font-size: 15px;
  font-weight: 800;
}
select {
  width: 275px;
  border: 2px solid #b9acac;
  border-radius: 4px;
  display: block;
  font-size: 14px;
  padding: 10px;
}
textarea {
  max-width: 275px;
  max-height: 100px;
}
button {
  background-color: #8294b9;
  border: 2px solid #8294b9;
  border-radius: 6px;
  height: 50px;
  display: block;
  font-size: 20px;
  cursor: pointer;
  transition: 0.8s;
}
p {
  margin: auto;
  font-size: 15px;
}
.successStatus {
  display: grid;
  width: 200px;
  height: 200px;
  color: rgb(255, 255, 255);
  background: rgb(66, 156, 66);
  border: 2px solid rgb(0, 128, 0);
  text-align: center;
  justify-content: center;
  border-radius: 50%;
  margin: 150px auto;
}
.successStatusParagraph {
  font-size: 25px;
}
</style>
