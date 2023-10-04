<template>
  <div class=" h-full flex w-full p-20 justify-center">
      <div class=" mx-auto lg:w-2/3 md:w-3/4 w-full h-1/2 text-center">
          <div class="text-gray-600 text-4xl">Simple Summation Calculator
              <span><button @click="modalOpen = true" class="ml-4 text-gray-500 hover:text-gray-900">ⓘ</button> </span>
          </div>

          <div class="w-full items-center justify-center pt-10">
              <form @submit="onSubmit">
                  <div class="items center">
                      <div class="flex-1">
                          <!-- <p>First Number</p> -->
                          <input id="first_input" v-model="first_input"
                              class="block w-full rounded-lg border-2 border-box mb-2 mr-2 pl-2 py-2" :class="{
                                  'border border-red-500': errors.first_input,
                              }" placeholder="First Number" />
                          <div class="text-red-400" v-if="errors.first_input">{{ errors.first_input }}</div>
                      </div>
                      <div class="p-4 text-3xl font-bold">
                          +
                      </div>
                      <div class="flex-1">
                          <!-- <p>Second Number</p> -->
                          <input id="second_input" v-model="second_input"
                              class="block w-full rounded-lg border-2 border-box mb-2 pl-2 py-2" :class="{
                                  'border border-red-500': errors.second_input,
                              }" placeholder="Second Number" />
                          <div class="text-red-400" v-if="errors.second_input">{{ errors.second_input }}</div>
                      </div>
                      <div class="p-4 text-3xl font-bold">
                      </div>
                      <div class="flex-1 w-full " >
                          <!-- <p>Second Number</p> -->
                          <button id="calculate"  :disabled="JSON.stringify(errors) !== '{}'"
                              class="block w-full rounded-lg  border-2 border-box mb-2 pl-2 py-2  hover:bg-green-200 disabled:bg-gray-300 disabled:opacity-50" >
                              Calculate
                      </button>
                          </div>
                  </div>
              </form>
              <div class="grid grid-cols-2 gap-3">
                  <button class="block w-full rounded-lg border-2 border-box mb-2 pl-2 py-2 hover:bg-green-200 disabled:bg-gray-300 disabled:opacity-50" :disabled='existingData.length === 0' @click="deleteRecord((existingData.length)-1)">Clear Recent Addition</button>
          <button class="block w-full rounded-lg border-2 border-box mb-2 pl-2 py-2 hover:bg-green-200 disabled:bg-gray-300 disabled:opacity-50" :disabled='existingData.length === 0' @click="deleteHistory">Clear History</button>    
      </div>
          </div>
          <div>
          <div class="grid grid-cols-12" v-for="(data, index) in existingData" :key="data.index">
              <span>{{ index + 1 }}.</span>
              <span class="col-span-3 overflow-x-auto  px-1">{{ data[0] }}</span>
              <span class=""> + </span>
              <span class="col-span-2 overflow-x-auto  px-1">{{ data[1] }}</span>
              <span class=""> = </span>
              <span class="col-span-3 overflow-x-auto px-1 ">{{ data[2] }}</span>
              <button @click="deleteRecord(index)" class="hover:text-indigo-500 hover:block hover:border-1 hover:border-box"> (x)Delete </button>
          </div>
      </div>
      </div>
  </div>
  <InformationModal v-if="modalOpen" :open="modalOpen" @close="modalOpen=false"></InformationModal>
</template>

<script setup>
import * as yup from "yup";
import { ref, onMounted } from 'vue';
import { useForm, useField } from "vee-validate";
const existingData = ref([]);
const modalOpen = ref(false);
import InformationModal from "./InformationModal.vue";
onMounted(() => {
  let data = localStorage.getItem('additionHistory');
  existingData.value = data ? JSON.parse(data) : [];
});

const deleteHistory = () => {
  localStorage.clear();
  let data = localStorage.getItem('additionHistory');
  existingData.value = data ? JSON.parse(data) : [];
}

const deleteRecord = (index) => {
  existingData.value.splice(index, 1);
  localStorage.setItem('additionHistory', JSON.stringify(existingData.value));
}
const AdditionSchema = yup.object().shape({
  first_input: yup.string().required("This field is required").matches(/^[0-9]+$/, 'Input is Restricted to Numeric Values Only'),
  second_input: yup.string().required("This field is required").matches(/^[0-9]+$/, 'Input is Restricted to Numeric Values Only'),
});
const { errors, handleSubmit } = useForm({
  validationSchema: AdditionSchema,
})
const { value: first_input } = useField("first_input");
const { value: second_input } = useField("second_input");

const onSubmit = handleSubmit(() => {


  let trimFirstInput = first_input.value.replace(/^0+/, '');
  let trimSecondInput = second_input.value.replace(/^0+/, '');
  const reversedString1 = trimFirstInput.split('').reverse().join('');
  const reversedString2 = trimSecondInput.split('').reverse().join('');


  let result = '';
  let carry = 0;

  // Determine the maximum length of the two strings
  const maxLength = Math.max(reversedString1.length, reversedString2.length);

  for (let i = 0; i < maxLength; i++) {
      const digit1 = parseInt(reversedString1[i]) || 0;
      const digit2 = parseInt(reversedString2[i]) || 0;

      const sum = digit1 + digit2 + carry;
      carry = Math.floor(sum / 10);
      const digitSum = sum % 10;

      result = digitSum.toString() + result;
  }

  // If there's a carry after all additions
  if (carry > 0) {
      result = carry.toString() + result;
  }
  const newItem = [trimFirstInput, trimSecondInput, result];
  // dataArray.push(newItem);
  const updatedData = [...existingData.value, newItem];
  localStorage.setItem('additionHistory', JSON.stringify(updatedData));
  let data = localStorage.getItem('additionHistory');
  existingData.value = data ? JSON.parse(data) : [];
  //    first_input.value = "";
  //    second_input.value = "";
  //    errors.value = {};
  //    event.target.reset();
});

</script>
