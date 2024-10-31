<template>
  <div class="regi_form">
    <b-form @submit.prevent="checkError" @submit="submitForm" required>
      <b-card header-tag="header" header-bg-variant="dark" header-text-variant="white">
        <template #header>
          <h4 class="mb-0" style="text-align: center">REGISTRATION FORM</h4>
        </template>
        <div
          v-for="formGroup in regisrationForm"
          :key="formGroup.id"
          v-on:keyup.enter="onEnter"
          style="margin: 0 30px; overflow: auto"
        >
          <component
            :is="formGroup.componentName"
            :item="formGroup"
            :key="twinKey"
            @addValue="addFormData"
          ></component>
        </div>
      </b-card>
    </b-form>
  </div>
</template>
<script>
import CommonLabel from '@/components/common/CommonLabel.vue'
import CommonTextInput from '@/components/common/CommonTextInput.vue'
import CommonDropdown from '@/components/common/CommonDropdown.vue'
import CommonRadio from '@/components/common/CommonRadio.vue'
import CommonTextArea from '@/components/common/CommonTextArea.vue'
import CommonCheck from '@/components/common/CommonCheck.vue'
import CommonButton from '@/components/common/CommonButton.vue'
import { FORM_PROPERTIES, FORM_OUTPUT } from '@/common/recipe/rRegistrationForm.js'

export default {
  name: 'RegistrationForm',
  components: {
    CommonTextInput,
    CommonLabel,
    CommonDropdown,
    CommonRadio,
    CommonTextArea,
    CommonCheck,
    CommonButton
  },
  /**
   * Object of data that has been submitted from the form is injected.
   */
  inject: ['twinShowData'],
  emits: ['addValue'],
  data() {
    return {
      regisrationForm: FORM_PROPERTIES,
      formData: FORM_OUTPUT,
      twinKey: 0,
      isError: true
    }
  },
  methods: {
    /**
     * Get the input data from the form
     * @param {array} data The data value of the inputs
     */
    addFormData(data) {
      this.formData[data.key] = data.value
    },
    /**
     * Submit the form
     */
    handleAdd(action) {
      if (action === 'submit') {
        this.submitForm()
      }
    },
    /**
     * Validate and display data if all requirements are satisfied.
     * @return {array} The array of inputs
     * @return {boolean} The value of isFavorite.
     * @return {boolean} The value of showDetails.
     * @return {boolean} The value of isError.
     */
    submitForm() {
      if (this.isError) {
        return
      }
      this.twinShowData.unshift({
        btnTwin: { ...JSON.parse(JSON.stringify(this.formData)) },
        isFavorite: false,
        showDetails: false
      })
      this.isError = true
      this.resetForm()
    },
    /**
     * Clears the form input
     */
    resetForm() {
      this.formData = {
        nickname: '',
        fullname: '',
        phone: '',
        email: '',
        birthdate: '',
        birthorder: '',
        coffee: '',
        lovelanguage: [],
        motto: ''
      }
      this.twinKey += 1
    },
    /**
     * Validates each of the data input
     * @param {number} min The minimum value
     * @param {number} max The maximum value
     * @param {string} text The text value
     * @return {number} The length of input
     */
    validateText(min, max, text) {
      const len = text.trim().length
      return len > min && len <= max
    },
    /**
     * Validates each of the data input
     */
    checkError() {
      const formData = this.formData
      const emailPattern = /^[a-z0-9._-]+@[a-z.-]+\.[a-zA-Z]{2,}$/

      const isValidNickname = this.validateText(0, 20, formData.nickname)
      const isValidFullname = this.validateText(0, 50, formData.fullname)
      const isValidEmail = emailPattern.test(formData.email.trim())
      const isValidBirthDate = this.validateText(0, 20, formData.birthdate)
      const isValidBirthOrder = this.validateText(0, 20, formData.birthorder)
      const isValidLoveLanguage = Object.keys(formData.lovelanguage).length === 3
      const isValidMotto = this.validateText(0, 1024, formData.motto)
      const isValidPhoneNumber = this.validateText(10, 12, formData.phone)

      if (!isValidNickname) alert(`Nickname should not be empty and exceed 20 characters.`)
      if (!isValidFullname) alert(`Fullname should not be empty and exceed 50 characters.`)
      if (!isValidEmail) alert('Invalid format')
      if (!isValidBirthDate) alert('Please enter your birth date')
      if (!isValidBirthOrder) alert('Please enter your birth order')
      if (!isValidLoveLanguage) alert('Please pick 3 love language')
      if (!isValidMotto) alert('Please enter your motto')
      if (!isValidPhoneNumber) alert('Please enter your phone number')

      if (
        isValidNickname &&
        isValidFullname &&
        isValidEmail &&
        isValidBirthDate &&
        isValidBirthOrder &&
        isValidLoveLanguage &&
        isValidMotto &&
        isValidPhoneNumber
      ) {
        this.isError = false
        alert('Submission Successful')
      }
    }
  }
}
</script>

<style scoped>
.regi_form {
  margin: 30px 0;
  height: 100%;
  overflow: hidden;
}
@media only screen and (max-width: 1440px) {
  .regi_form {
    margin: auto;
    padding: 30px 0;
  }
}
</style>
