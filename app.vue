<script setup>
import { useVuelidate } from '@vuelidate/core';
import { required, requiredIf, minLength, minValue, maxValue, helpers } from '@vuelidate/validators';

const formData = reactive({
  day: '',
  month: '',
  year: '',
});

const validDate = () => {
  const date = new Date(`${formData.year}-${formData.month}-${formData.day}`);
  let tempDay = date.getDate().toString()
  if(tempDay.length === 1) tempDay = `0${tempDay}`
  return tempDay === formData.day || formData.year === '' || formData.month === '' || formData.day === '';
}

const rules = computed(() => {
  return {
    day: {
      required: helpers.withMessage('This field is required', required),
      minLength: helpers.withMessage('Please add leading 0s', minLength(2)),
      minValue: helpers.withMessage('Must be a valid day', minValue('01')),
      maxValue: helpers.withMessage('Must be a valid day', maxValue('31')),
      validDate: helpers.withMessage('Must be a valid date', validDate)
    },
    month: {
      required: helpers.withMessage('This field is required', required),
      minLength: helpers.withMessage('Please add leading 0s', minLength(2)),
      minValue: helpers.withMessage('Must be a valid month', minValue('01')),
      maxValue: helpers.withMessage('Must be a valid month', maxValue('12')),
      validDate
    },
    year: {
      required: helpers.withMessage('This field is required', required),
      minLength: helpers.withMessage('Must be a valid year', minLength(4)),
      maxValue: helpers.withMessage('Must be in the past', maxValue(new Date().getFullYear().toString())),
      validDate
    },
  };
});

const v$ = useVuelidate(rules, formData);

const years = ref(null)
const months = ref(null)
const days = ref(null)

function calculate() {
  let now = new Date()
  let then = new Date(`${formData.year}-${formData.month}-${formData.day}`)
  years.value = now.getUTCFullYear() - then.getUTCFullYear()
  months.value = now.getMonth() - then.getMonth()
  if (then.getDate() > now.getDate()) {
    months.value -= 1
  }
  if (months.value < 0) {
    months.value += 12
    years.value -= 1
  }
  days.value = 0
  while (then.getDate() > now.getDate() && then.getDate() !== 1) {
    then.setDate(then.getDate() + 1)
    days.value += 1
    console.log(then.getDate())
  }
  days.value += parseInt(now.getDate() - then.getDate())
}
</script>

<template>
  <div id="app-container"
    class="text-base font-normal font-poppins bg-[--off-white] w-screen h-screen flex items-center justify-center">
    <div class="bg-white p-10 rounded-xl rounded-br-[25%] flex flex-col gap-1 max-w-[900px] w-[75%]">
      <div class="flex flex-row gap-5">
        <div class="flex flex-col">
          <label class="text-[0.5rem] font-bold tracking-[0.15rem]" 
          :class="{'text-[--light-red]': v$.day.$error, 'text-[--smokey-grey]': !v$.day.$error}">DAY</label>
          <input
            class="w-20 py-1 px-2.5 border border-[--light-grey] rounded-[5px] text-[--off-black] font-bold focus:border-[--purple] focus:outline-none"
            placeholder="DD" maxlength="2" v-model="formData.day"
            :class="{'border-[--light-red]': v$.day.$error}" />
          <p class="text-[14px] italic text-[--light-red]" v-if="v$.day.$error">{{ v$.day.$errors[0].$message }}</p>
        </div>
        <div class="flex flex-col">
          <label class="text-[0.5rem] font-bold tracking-[0.15rem]"
          :class="{'text-[--light-red]': v$.month.$error, 'text-[--smokey-grey]': !v$.month.$error}">MONTH</label>
          <input
            class="w-20 py-1 px-2.5 border border-[--light-grey] rounded-[5px] text-[--off-black] font-bold focus:border-[--purple] focus:outline-none"
            placeholder="MM" maxlength="2" v-model="formData.month"
            :class="{'border-[--light-red]': v$.month.$error}" />
          <p class="text-[14px] italic text-[--light-red]" v-if="v$.month.$error">{{ v$.month.$errors[0].$message }}</p>
        </div>
        <div class="flex flex-col">
          <label class="text-[0.5rem] font-bold tracking-[0.15rem]"
          :class="{'text-[--light-red]': v$.year.$error, 'text-[--smokey-grey]': !v$.year.$error}">YEAR</label>
          <input
            class="w-20 py-1 px-2.5 border border-[--light-grey] rounded-[5px] text-[--off-black] font-bold focus:border-[--purple] focus:outline-none"
            placeholder="YYYY" maxlength="4" v-model="formData.year"
            :class="{'border-[--light-red]': v$.year.$error}" />
          <p class="text-[14px] italic text-[--light-red]" v-if="v$.year.$error">{{ v$.year.$errors[0].$message }}</p>
        </div>
      </div>
      <div class="flex flex-row relative items-center">
        <div class="h-[1px] w-full bg-[--light-grey]" />
        <button :class="{ 'p-3 bg-[--off-black] rounded-full w-fit ml-auto': true, 'bg-[--purple]': years !== null }"
          @click="() => {
              v$.$validate();
              if(!v$.$invalid) {
                calculate();
                v$.$reset();
              }
            }">
          <img src="/assets/images/icon-arrow.svg" />
        </button>
      </div>
      <div class="font-extrabold italic text-5xl"><span class="text-[--purple]">{{ years === null ? '--' : years
          }}</span> years</div>
      <div class="font-extrabold italic text-5xl"><span class="text-[--purple]">{{ months === null ? '--' : months
          }}</span> months</div>
      <div class="font-extrabold italic text-5xl"><span class="text-[--purple]">{{ days === null ? '--' : days }}</span>
        days</div>
    </div>
  </div>
</template>
