<template>
  <div class="formDiv">
    <form @submit.prevent="veeSubmitForm">
      <h1>Calculating Form</h1>
      <div class="form-group">
        <div class="radioButton">
          <p>
            {{ form.descriptionMaxLength - form.description.length }} /
            {{ form.descriptionMaxLength }}
          </p>
        </div>

        <label for="Description"
          >Description
          <textarea
            v-model="form.description"
            @blur="v$.form.description.$touch"
            maxlength="255"
          >
          </textarea>
          <div class="errors" v-if="v$.form.description.$error">
            Text is required
          </div>
        </label>
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
          <option  value  disabled>Choose vat</option>
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
        <input
          type="number"
          v-model="form.nettoPriceInput"
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
        <div class="displayFlex">
          <label for="yes"
            >Yes
            <input
              type="radio"
              name="yesOrNo"
              v-model="form.radioButton"
              @blur="v$.form.vatInput.$touch"
            />
          </label>
        </div>
        <div class="displayFlex">
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
        <div class="errors" v-if="v$.form.radioButton.$error">
          Text is required
        </div>
      </div>
      <button type="submit">Submit</button>
    </form>
  </div>
</template>

<script>
import useVuelidate from "@vuelidate/core";
import { required } from "@vuelidate/validators";

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
      form: {
        description: "",
        vatInput: "",
        radioButton: null,
        bruttoPriceInput: null,
        nettoPriceInput: null,
        descriptionMaxLength: 255,
      },
    };
  },
  validations() {
    return {
      description2: {
        required,
      },
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
      // you can show some extra alert to the user or just leave the each field to show it's `$errors`.
      if (!isFormCorrect) return;
      // actually submit form
    },
    submitForm() {
      this.$v.form.$touch();
      if (!this.$v.form.$invalid) {
        console.log("📝 Form Submitted", this.form);
      } else {
        console.log("❌ Invalid form");
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
      return !!this.form.radioButton;
    },
    chooseVatValidation() {
      return !!this.form.vatInput;
    },
    nettoPriceValidation() {
      return !!this.form.nettoPriceInput;
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
textarea {
  width: 250px;
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
select {
  width: 275px;
  border: 2px solid #b9acac;
  border-radius: 4px;
  display: block;
  font-size: 14px;
  padding: 10px;
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
</style>
