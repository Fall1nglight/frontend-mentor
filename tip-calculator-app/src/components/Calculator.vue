<template>
  <div class="container py-5 p-sm-5">
    <div class="text-center mb-5">
      <img src="../images/logo.svg" alt="" class="img-fluid" />
    </div>
    <div class="row p-5 justify-content-evenly gx-5" id="mainRow">
      <!-- User-setting panel -->
      <div class="col-md-6 col-lg-5 mb-5 mb-md-0">
        <form>
          <p class="lead mb-2">Bill</p>
          <div class="input-group mb-4">
            <span class="input-group-text"
              ><img src="../images/icon-dollar.svg" alt=""
            /></span>
            <input
              v-model="bill"
              type="number"
              class="form-control"
              id="billInput"
              aria-label="Amount (to the nearest dollar)"
            />
          </div>

          <p class="lead mb-2">Select Tip %</p>
          <div class="row row-cols-2 row-cols-lg-3 g-3 text-center mb-4">
            <!-- Item-1 -->
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

            <!-- Item-2 -->
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

            <!-- Item-3 -->
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

            <!-- Item-4 -->
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

            <!-- Item-5 -->
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

            <!-- Item-6 -->
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

          <p class="lead mb-2">Number of People</p>
          <div class="input-group">
            <span class="input-group-text"
              ><img src="../images/icon-person.svg" alt=""
            /></span>
            <input
              v-model="numOfPeople"
              type="number"
              id="numOfPeopleInput"
              class="form-control"
              aria-label="Number of people who would like to pay"
              min="1"
            />
          </div>
        </form>
      </div>

      <!-- Display-panel -->
      <div class="col-md-6 col-lg-5 py-5 py-sm-0" id="dataDisplay">
        <div class="row px-2 px-sm-4 my-0 my-sm-5 mb-5">
          <div class="col-6">
            <p class="lead mb-0 text-white">Tip Amount</p>
            <span class="person">/ person</span>
          </div>
          <div class="col-6 h3 text-end">
            {{ '$' + Math.round(tipAmountPerPerson * 100) / 100 }}
          </div>
        </div>

        <div class="row px-2 px-sm-4 mb-4 mb-lg-5">
          <div class="col-6">
            <p class="lead mb-0 text-white">Total</p>
            <span class="person">/ person</span>
          </div>
          <div class="col-6 h3 text-end">
            {{ '$' + Math.round(totalPerPerson * 100) / 100 }}
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
    const total = computed(
      () => bill.value + (bill.value / 100) * tipPercentage.value
    );

    const tipAmountPerPerson = computed(() => {
      if (!tipPercentage.value) return 0;
      return ((bill.value / 100) * tipPercentage.value) / numOfPeople.value;
    });

    const totalPerPerson = computed(() => total.value / numOfPeople.value);

    const reset = () => {
      bill.value = '';
      numOfPeople.value = 1;
      tipPercentage.value = 0;
      removeSelectedClasses();
    };

    //functions
    const setPercentage = (event) => {
      tipPercentage.value = event.srcElement.value;

      for (let i = 1; i <= 5; i++) {
        const labelName = `option${i}`;

        if (labelName === event.srcElement.id) continue;
        document
          .querySelector(`[for=${labelName}`)
          .classList.remove('selected');
      }

      document
        .querySelector(`[for=${event.srcElement.id}]`)
        .classList.add('selected');
    };

    const setCustomPercentage = (event) => {
      tipPercentage.value = event.srcElement.value;
      removeSelectedClasses();
    };

    const removeSelectedClasses = () => {
      for (let i = 1; i <= 5; i++) {
        document.querySelector(`[for=option${i}`).classList.remove('selected');
      }
    };

    /* TODO
    - {{}} clearout
    - css átrendezés
    - js refactoring
    - html clearout margin / padding / responsive stuff
     */

    return {
      bill,
      setPercentage,
      setCustomPercentage,
      numOfPeople,
      totalPerPerson,
      tipAmountPerPerson,
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

.h3 {
  font-weight: 700;
  color: hsl(172, 67%, 45%) !important;
  overflow-wrap: break-word;
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

.input-group:hover span,
.input-group:hover input[type='number'],
#customTipInput:hover {
  border-color: hsl(172, 67%, 45%);
}

#billInput,
#numOfPeopleInput {
  border-left: 0;
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

.lead {
  color: hsl(186, 14%, 43%);
  font-weight: 700;
  font-size: 1rem;
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
