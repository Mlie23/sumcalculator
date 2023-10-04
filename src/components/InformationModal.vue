<template>
    <TransitionRoot :show="props.open">
      <Dialog @close="emit('close')" class="fixed">
        <DialogPanel>
          <div
            class="fixed inset-0 z-10 flex w-full items-center justify-center py-12 bg-gray-500/[0.8]"
          >
            <div class="w-full fixed sm:mx-auto sm:max-w-2xl">
              <DialogPanel class="relative">
                <div
                  class="rounded shadow-2xl bg-white text-black m-5 sm:m-0 border-2 border-gray-400"
                >
                  <div class="w-full h-8">
                    <button
                      type="close"
                      @click="emit('close')"
                      class="sm:m-0 float-right w-8 h-8"
                    >
                     X
                    </button>
                  </div>
  
  
                    <div class="pt-1 px-6 pb-6">
                      <h1 class="text-3xl mb-4 text-center">Calculator Information</h1>
                      <ol class="list-decimal">
                        <li>This app was created to showcase my skill and competency in Laravel, Vue.JS, and Tailwind.CSS.</li>
                        <li><span class="underline hover:text-gray-700 hover:cursor-default">First Number</span> and <span class="underline hover:text-gray-700 hover:cursor-default">Second Number</span> needs to be filled to be able to see the result and added to the calculator history.</li>
                        <li>There are three main functions of this calculator:
                            <ol class="list-decimal pl-4">
                                <li >Calculate -> Will be executable if both the fields are filled with no error validations triggered.</li>
                                <li>Clear Addition -> Will clear recent calculation from the calculator history.</li>
                                <li>Clear History -> Completely remove the calculation history.</li>
                            </ol>
                        </li>
                        <li>Validation was created using yup and vee-validate.</li>
                        <li>This Modal was created using vue HeadlessUI.</li>
                      </ol>
  
  

                    </div>
                    &copy; Michael Lie - 2023
                </div>
              </DialogPanel>
              <!-- </div> -->
            </div>
          </div>
        </DialogPanel>
      </Dialog>
    </TransitionRoot>
  </template>
  
  <script setup>
  /* eslint-disable */ 
  import {
  TransitionRoot,
  Dialog,
  DialogPanel,
} from "@headlessui/vue";
import { defineEmits } from 'vue';
const props = defineProps({
  open: { type: Boolean, required: true },
});
const emit = defineEmits(['close']);


</script>