<template>
  <div class="container py-5 p-sm-5">
    <div class="text-center mb-5">
      <img src="../images/logo.svg" alt="" class="img-fluid" />
    </div>
    <div class="row p-5 justify-content-evenly gx-5" id="mainRow">
      <!-- User input -->
      <div class="col-md-6 col-lg-5 mb-5 mb-md-0">
        <form>
          <p class="lead mb-2 d-flex align-items-center">
            Bill
            <span v-if="billErrorMsg" class="ms-auto text-danger error-msg">
              {{ billErrorMsg }}
            </span>
          </p>
          <div class="input-group mb-4">
            <span
              :class="[billErrorMsg ? 'border-danger' : '', 'input-group-text']"
              ><img src="../images/icon-dollar.svg" alt="Dollar icon"
            /></span>
            <input
              v-model="bill"
              type="number"
              :class="[billErrorMsg ? 'border-danger' : '', 'form-control']"
              id="billInput"
              aria-label="Amount in dollars"
            />
          </div>

          <p class="lead mb-2">Select Tip %</p>
          <div class="row row-cols-2 row-cols-lg-3 g-3 text-center mb-4">
            <!-- Input-1 -->
            <div class="col">
              <input
                @click="setPercentage"
                type="radio"
                class="btn-check"
                name="options"
                id="option1"
                value="5"
                autocomplete="off"
              />
              <label class="btn custom-btn w-100" for="option1">5%</label>
            </div>

            <!-- Input-2 -->
            <div class="col">
              <input
                @click="setPercentage"
                type="radio"
                class="btn-check"
                name="options"
                id="option2"
                value="10"
                autocomplete="off"
              />
              <label class="btn custom-btn w-100" for="option2">10%</label>
            </div>

            <!-- Input-3 -->
            <div class="col">
              <input
                @click="setPercentage"
                type="radio"
                class="btn-check"
                name="options"
                id="option3"
                value="15"
                autocomplete="off"
              />
              <label class="btn custom-btn w-100" for="option3">15%</label>
            </div>

            <!-- Input-4 -->
            <div class="col">
              <input
                @click="setPercentage"
                type="radio"
                class="btn-check"
                name="options"
                id="option4"
                value="25"
                autocomplete="off"
              />
              <label class="btn custom-btn w-100" for="option4">25%</label>
            </div>

            <!-- Input-5 -->
            <div class="col">
              <input
                @click="setPercentage"
                type="radio"
                class="btn-check"
                name="options"
                id="option5"
                value="50"
                autocomplete="off"
              />
              <label class="btn custom-btn w-100" for="option5">50%</label>
            </div>

            <!-- Input-6 -->
            <div class="col">
              <input
                @input="setCustomPercentage"
                type="number"
                class="form-control text-light"
                id="customTipInput"
                placeholder="Custom"
              />
            </div>
          </div>

          <p class="lead mb-2 d-flex align-items-center">
            Number of People
            <span
              class="ms-auto text-danger error-msg"
              v-if="numOfPeopleErrorMsg"
            >
              {{ numOfPeopleErrorMsg }}
            </span>
          </p>
          <div class="input-group mb-2 mb-lg-0">
            <span
              :class="[
                numOfPeopleErrorMsg ? 'border-danger' : '',
                'input-group-text',
              ]"
              ><img src="../images/icon-person.svg" alt=""
            /></span>
            <input
              v-model="numOfPeople"
              type="number"
              id="numOfPeopleInput"
              :class="[
                numOfPeopleErrorMsg ? 'border-danger' : '',
                'form-control',
              ]"
              aria-label="Number of people who would like to pay"
            />
          </div>
        </form>
      </div>

      <!-- Data display -->
      <div class="col-md-6 col-lg-5 py-5 py-sm-0" id="dataDisplay">
        <div class="row px-2 px-sm-4 my-0 my-sm-5 mb-5">
          <div class="col-6">
            <p class="lead mb-0 text-white">Tip Amount</p>
            <span class="person">/ person</span>
          </div>
          <div class="col-6 h3 text-end">
            {{ `$${tipAmountPerPerson}` }}
          </div>
        </div>

        <div class="row px-2 px-sm-4 mb-4 mb-lg-5">
          <div class="col-6">
            <p class="lead mb-0 text-white">Total</p>
            <span class="person">/ person</span>
          </div>
          <div class="col-6 h3 text-end">
            {{ `$${totalPerPerson}` }}
          </div>
        </div>

        <div class="text-center pb-3">
          <button @click="reset" class="btn w-75" id="reset">
            Reset
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { ref, computed } from 'vue';

export default {
  name: 'Calculator',

  setup() {
    //refs
    const bill = ref('');
    const numOfPeople = ref(1);
    const tipPercentage = ref(0);

    //computed properties
    const tipAmountPerPerson = computed(() => {
      if (!tipPercentage.value || numOfPeople.value < 1) return 0;
      return (
        Math.round(
          (((bill.value / 100) * tipPercentage.value) / numOfPeople.value) * 100
        ) / 100
      );
    });

    const totalPerPerson = computed(() => {
      if (numOfPeople.value < 1 || bill.value < 1) return 0;
      return (
        Math.round(
          ((bill.value + (bill.value / 100) * tipPercentage.value) /
            numOfPeople.value) *
            100
        ) / 100
      );
    });

    const billErrorMsg = computed(() => {
      if (bill.value === '') return '';
      if (bill.value < 1) return "Can't be zero or less";
    });

    const numOfPeopleErrorMsg = computed(() =>
      numOfPeople.value < 1 ? "Can't be zero or less" : ''
    );

    //functions
    const setPercentage = ({
      srcElement: { value: inputValue },
      srcElement: { id },
    }) => {
      tipPercentage.value = inputValue;

      for (let i = 1; i <= 5; i++) {
        const labelName = `option${i}`;

        if (labelName === id) continue;

        document
          .querySelector(`[for=${labelName}`)
          .classList.remove('selected');
      }

      document.querySelector(`[for=${id}]`).classList.add('selected');

      document.getElementById('customTipInput').value = '';
    };

    const setCustomPercentage = ({ srcElement: { value: inputValue } }) => {
      if (inputValue < 0) return;
      tipPercentage.value = inputValue;
      removeSelectedClasses();
    };

    const reset = () => {
      bill.value = '';
      numOfPeople.value = 1;
      tipPercentage.value = 0;
      removeSelectedClasses();
    };

    const removeSelectedClasses = () => {
      for (let i = 1; i <= 5; i++) {
        document.querySelector(`[for=option${i}`).classList.remove('selected');
      }
    };

    return {
      bill,
      billErrorMsg,
      setPercentage,
      setCustomPercentage,
      numOfPeople,
      numOfPeopleErrorMsg,
      tipAmountPerPerson,
      totalPerPerson,
      reset,
    };
  },
};
</script>

<style scoped>
#mainRow {
  background: white;
  border-radius: 25px;
  box-shadow: 1px 1px 10px black;
}

#dataDisplay {
  background-color: hsl(183, 100%, 15%);
  color: white;
  border-radius: 25px;
}

.lead {
  color: hsl(186, 14%, 43%);
  font-weight: 700;
  font-size: 1rem;
}
.error-msg {
  font-size: 0.8rem;
}

input[type='number'] {
  color: hsl(183, 100%, 15%) !important;
  font-weight: 700;
  background-color: hsl(185, 41%, 84%);
  transition-property: none;
}

input[type='number']:hover {
  cursor: pointer;
}

#billInput,
#numOfPeopleInput {
  border-left: 0;
}

.custom-btn {
  background-color: hsl(183, 100%, 15%);
  color: white;
  font-weight: 700;
}

.custom-btn:hover {
  background-color: hsl(172, 67%, 75%);
  border-color: hsl(172, 67%, 75%);
  color: hsl(183, 100%, 15%);
}

.input-group:hover span,
.input-group:hover input[type='number'],
#customTipInput:hover {
  border-color: hsl(172, 67%, 45%);
}

#customTipInput {
  background-color: hsl(185, 41%, 84%);
  text-align: center;
}

#customTipInput::placeholder {
  text-align: center;
  color: hsl(186, 14%, 43%);
  font-weight: 700;
}

.h3 {
  font-weight: 700;
  color: hsl(172, 67%, 45%) !important;
  overflow-wrap: break-word;
}

.person {
  color: hsl(186, 14%, 43%);
  font-weight: 700;
}

#reset {
  background-color: hsl(172, 67%, 45%);
  font-weight: 700;
  letter-spacing: 2px;
  text-transform: uppercase;
  color: hsl(183, 100%, 15%);
}

#reset:hover {
  background-color: hsl(172, 67%, 75%);
}

.selected {
  background-color: hsl(172, 67%, 45%) !important;
  border-color: hsl(172, 67%, 45%) !important;
  color: hsl(183, 100%, 15%) !important;
}
</style>
