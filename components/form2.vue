<template>
  <ValidationObserver v-slot="{ invalid }">
      <form @submit.prevent="doAction()"> 
        <ValidationProvider
          name="номер телефона"
          :rules="{ required: true, regex: /^\+\d{1}\(\d{3}\)\s\d{3}[-]\d{2}[-]\d{2}$/}"
          tag="div"
          class="field"
        >
          <template slot-scope="{ errors }">
            <label for="number_data">
              Номер телефона
            </label>
            <input id="number_data" 
            v-model="numberData" 
            v-mask="'+7(###) ###-##-##'" 
            placeholder="+7(000) 000-00-00" 
            type="text">
           <div class="error" v-if="errors[0]">
              {{ errors[0] }}
            </div>
          </template>
        </ValidationProvider>

        <ValidationProvider
          name="имя"
          :rules="{ required: true, alpha: true, max: 10 }"
          :bails="false"
          tag="div"
          class="field"
        >
          <template slot-scope="{ errors }">
            <label for="name">
              Имя
            </label>
            <input id="name" type="text" v-model="name" >
            <div 
              class="error" 
              v-for="error in errors" 
              :key="error"
             >
               {{ error }}
            </div>
          </template>
        </ValidationProvider>

        <ValidationProvider 
             name="checkbox"
             rules="required" >
            <v-checkbox 
              input-value="true" 
              on-icon="fa-square-filled" 
              off-icon="fa-square-empty" 
              indeterminate-icon="fa-something-else"
              slot-scope="{
                errors,
                valid
              }"
              :error-messages="errors"
              :success="valid"
              v-model="checkbox1"
              label="Согласен на обработку персональных данных"
              type="checkbox"
              required
            ></v-checkbox>
          </ValidationProvider>

        <button type="submit" :disabled="invalid || validated">
          Отправить форму
        </button>
      </form>

  </ValidationObserver>
</template>

<script>
import { ValidationProvider, ValidationObserver } from "vee-validate";
import Vue from 'vue';
import VueMask from 'v-mask';
import Vuetify from "vuetify";
Vue.use(Vuetify);
import VeeValidate from "vee-validate";
Vue.use(VeeValidate);
import PrettyCheckbox from 'pretty-checkbox-vue';
Vue.use(PrettyCheckbox);
Vue.use(VueMask);

export default {
  components: {
    ValidationProvider,
    ValidationObserver,
    VueMask,
    PrettyCheckbox,
    Vuetify,
    },

  data() {
    return {
      numberData: null,
      name: null,
      checkbox: "",
      checkbox1: true,
    };
  },

  methods: {
    doAction() {
      alert('Форма отправлена');
      this.numberData =null
      this.name =null
    },
  },
};
</script>

<style scoped>
.field {
  margin-bottom: 24px;
}

.field > label {
  margin-right: 8px;
}

.error {
  color: red;
}
</style>