<template>
  <div class="bg-gray-50">
    <div class="mx-auto max-w-2xl px-4 pb-24 pt-16 sm:px-6 lg:max-w-7xl lg:px-8">
      <UNotification icon="i-heroicons-x-circle" id="paygate-warning" class="text-center" color="red" :timeout="0" title="This page does not actually process any payments. It only simulates the flow between the payment system and an ecommerce/transaction system" />

      <form class="lg:grid lg:grid-cols-2 lg:gap-x-12 xl:gap-x-16" method="POST" action="/api/pay">
        <div>
          <!-- Payment -->
          <div class="mt-10 border-t border-gray-200 pt-10">
            <h2 class="text-lg font-medium text-gray-900">Payment</h2>
            <input type="hidden" v-model="amount" id="amount" name="amount"/>
            <input type="hidden" v-model="currency" id="currency" name="currency"/>
            <input type="hidden" v-model="orderId" id="orderId" name="orderId"/>
            <input type="hidden" v-model="callbackAccept" id="callbackAccept" name="callbackAccept"/>
            <input type="hidden" v-model="calllbackFailure" id="calllbackFailure" name="calllbackFailure"/>

            <div class="mt-6 grid grid-cols-4 gap-x-4 gap-y-6">
              <div class="col-span-4">
                <label for="card-number" class="block text-sm font-medium text-gray-700">Card number</label>
                <div class="mt-1">
                  <input type="text" id="card-number" name="card-number" v-model="cardNumber" autocomplete="cc-number" class="block w-full rounded-md border-gray-300 shadow-sm focus:border-indigo-500 focus:ring-indigo-500 sm:text-sm" />
                </div>
              </div>

              <div class="col-span-4">
                <label for="name-on-card" class="block text-sm font-medium text-gray-700">Name on card</label>
                <div class="mt-1">
                  <input type="text" id="name-on-card" name="name-on-card" v-model="cardName" autocomplete="cc-name" class="block w-full rounded-md border-gray-300 shadow-sm focus:border-indigo-500 focus:ring-indigo-500 sm:text-sm" />
                </div>
              </div>

              <div class="col-span-3">
                <label for="expiration-date" class="block text-sm font-medium text-gray-700">Expiration date (MM/YY)</label>
                <div class="mt-1">
                  <input type="text" name="expiration-date" id="expiration-date" v-model="cardExpiration" autocomplete="cc-exp" class="block w-full rounded-md border-gray-300 shadow-sm focus:border-indigo-500 focus:ring-indigo-500 sm:text-sm" />
                </div>
              </div>

              <div>
                <label for="cvc" class="block text-sm font-medium text-gray-700">CVC</label>
                <div class="mt-1">
                  <input type="text" name="cvc" id="cvc" v-model="cardChecksum" autocomplete="csc" class="block w-full rounded-md border-gray-300 shadow-sm focus:border-indigo-500 focus:ring-indigo-500 sm:text-sm" />
                </div>
              </div>
            </div>
            <div class="border-t border-gray-200 px-4 py-6 sm:px-6">
              <UButton block size="lg" variant="solid" color="indigo" :label="'Pay ' + currency + ' ' + (amount / 100).toFixed(2)" icon="i-heroicons-credit-card"/>
            </div>
          </div>
        </div>
      </form>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import { RadioGroup, RadioGroupDescription, RadioGroupLabel, RadioGroupOption } from '@headlessui/vue'

const cardNumber = ref('')
const cardName = ref('')
const cardExpiration = ref('')
const cardChecksum = ref('')

const amount = ref(0.0) // Required
const currency = ref('USD') // Required
const orderId = ref('') // Required
const callbackAccept = ref('/payment/generic/accept') // Required
const calllbackFailure = ref('/payment/generic/failure') // Required

const params = useRoute().query

if (params) {
    amount.value = params.amount || 0.0
    currency.value = params.currency || 'USD'
    orderId.value = params.order || ''
    callbackAccept.value = params.acceptUrl || '/payment/generic/accept'
    calllbackFailure.value = params.failureUrl || '/payment/generic/failure'
}

</script>