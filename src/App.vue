<template>
  <div id="app">
    <form @submit.prevent="calculate()">
      <div class="formGroup">
        <AddressInput
          name="oct1"
          v-model="oct1"
          maxlen="3"
          :class="{ error: errors.has('oct1')}"
          v-validate="'required|min:1|max_value:255|min_value:0'"
        />.
        <AddressInput
          name="oct2"
          v-model="oct2"
          maxlen="3"
          :class="{ error: errors.has('oct2')}"
          v-validate="'required|min:1|max_value:255|min_value:0'"
        />.
        <AddressInput
          name="oct3"
          v-model="oct3"
          maxlen="3"
          :class="{ error: errors.has('oct3')}"
          v-validate="'required|min:1|max_value:255|min_value:0'"
        />.
        <AddressInput
          name="oct4"
          v-model="oct4"
          maxlen="3"
          :class="{ error: errors.has('oct4')}"
          v-validate="'required|min:1|max_value:255|min_value:0'"
        />/
        <AddressInput
          name="cidr"
          v-model="cidr"
          maxlen="2"
          :class="{ error: errors.has('cidr')}"
          v-validate="'required|min:1|max_value:32|min_value:0'"
        />
      </div>
      <div class="formGroup controls">
        <ControlButton text="Calculate" role="submit"/>
        <ControlButton text="Clear" role="reset"/>
      </div>
    </form>
    <div class="output">
      <p> Network: {{ netAddress }} </p>
      <p> Subnet mask: {{ netMask }} </p>
      <p> First host: {{ firstHost }} </p>
      <p> Last host: {{ lastHost }} </p>
      <p> Broadcast: {{ boradcast }} </p>
    </div>
  </div>
</template>

<script>
import ControlButton from './components/ControlButton'
import AddressInput from './components/AddressInput'

export default {
  name: 'app',
  components: {
    ControlButton,
    AddressInput
  },
  data() {
    return {
      oct1: '',
      oct2: '',
      oct3: '',
      oct4: '',
      cidr: '',

      netAddress: '',
      netMask: '',
      firstHost: '',
      lastHost: '',
      boradcast: '',

      rawNetaddress: '',
      rawMask: '',
    }
  },
  methods: {
    calculateNetAddress() {

    },
    calculateNetmask() {
      let mask = '';

      for (let i = 0; i < this.cidr; i++)
        this.rawMask += '1';

      for (let i = 0; i < 32 - this.cidr; i++)
        this.rawMask += 0;


      for (let i = 0, j = 0; i < 4; i++, j += 8) {
        mask += parseInt(this.rawMask.substring(j, j + 8), 2);
        if (i < 3) {
          mask += '.';
        }
      }

      this.netMask = mask;
    },
    calculate() {
      this.$validator.validateAll().then((result) => {
        if (result) {
          this.rawNetaddress = '';
          this.rawMask = '';

          this.calculateNetmask();
          this.calculateNetAddress();

          // hide keyboard
          document.getElementsByTagName('body')[0].focus();
        }
      });
    },
    clear() {
      // focus first form element after clear
      document.getElementsByClassName('addressInput')[0].focus();
    }
  }
}
</script>

<style >
  @import url('https://fonts.googleapis.com/css?family=Montserrat');

  *,
  *::before,
  *::after {
    box-sizing: border-box;
    font-family: 'Montserrat', sans-serif;
  }

  p {
    margin: .5em 0;
  }

  #app {
    padding: 20px;
    max-width: 380px;
    margin-left: auto;
    margin-right: auto;
  }

  .formGroup {
    display: flex;
    justify-content: space-between;
    margin: 20px auto;
    align-items: center;
  }

  .formGroup .addressInput {
    height: 40px;
    max-width: 55px;
    border: none;
    border-bottom: solid 1px #000;
    text-align: center;
    font-size: 1.5em;
  }

  .error {
    border-bottom: solid 2px #f00 !important;
  }

  .formGroup .addressInput:focus {
    outline: none;
  }

  .formGroup.controls {
    flex-direction: column;
    padding-top: 20px;
  }

  .output {
    margin-top: 30px;
  }
</style>
