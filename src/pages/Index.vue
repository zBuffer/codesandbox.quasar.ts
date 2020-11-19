<template>
  <q-page class="row items-center justify-evenly">
    <q-card>
      <q-form ref="form" @submit="generate" @reset="clear">
        <q-card-section>
          <div class="text-h6">Quasar Proverb Generator</div>
          <div class="text-subtitle">To create a proverb, fill in Noun and Adjective, then click Generate</div>
        </q-card-section>
        <q-card-section class="q-pt-none">
          <div class="row q-col-gutter-md">
            <q-input 
              v-model="noun" 
              label="Noun" class="col-6"
              :rules="[ generateValidationFn('noun') ]"
            ></q-input>
            <q-input 
              v-model="adjective" 
              label="Adjective" class="col-6"
              :rules="[ generateValidationFn('adjective') ]"
            ></q-input>
          </div>
        </q-card-section>
        <q-card-actions align="right">
          <q-btn flat color="primary" type="submit">Submit</q-btn>
          <q-btn flat type="reset">Clear</q-btn>
        </q-card-actions>
      </q-form>
    </q-card>
    <q-dialog v-model="isAlertShown" @keypress.enter="isAlertShown = false">
      <q-card>
        <q-card-section>
          <div class="text-subtitle2">Here's Your Proverb</div>
        </q-card-section>
        <q-card-section class="q-pt-none">
          <div class="text-h5">Strike while the {{ noun }} is {{ adjective }}</div>
        </q-card-section>
        <q-card-actions align="right">
          <q-btn flat color="primary" v-close-popup>OK</q-btn>
        </q-card-actions>
      </q-card>
    </q-dialog>
  </q-page>
</template>

<script lang="ts">
import { defineComponent, ref } from '@vue/composition-api';
import { QForm } from 'quasar';

export default defineComponent({
  name: 'PageIndex',
  setup() {
    let noun      = ref(''),
        adjective = ref(''),
        isAlertShown = ref(false);

    let form = ref<QForm | null>(null);

    return {
      noun,
      adjective,
      isAlertShown,
      form,
      generate,
      generateValidationFn,
      clear
    };

    /**
     * Handles form submit event
     */
    async function generate() {
      isAlertShown.value = await validateForm();
    }
    /**
     * Calls form validation function
     */
    async function validateForm() {
      return await form.value?.validate(true) === true;
    }
    /**
     * Handles form reset event
     */
    function clear() {
      [ noun, adjective ].forEach(p => p.value = '');
    }
    /**
     * Generates validation function
     * @param key name of field
     */
    function generateValidationFn(key: string) {
      return (v: string | number) => !!v || `Please enter ${key}`;
    }
  }
});
</script>
