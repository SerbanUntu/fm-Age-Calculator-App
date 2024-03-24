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
      required:  helpers.withMessage('This field is required', required),
      minLength: helpers.withMessage('Please add leading 0s', minLength(2)),
      minValue:  helpers.withMessage('Must be a valid day', minValue('01')),
      maxValue:  helpers.withMessage('Must be a valid day', maxValue('31')),
      validDate: helpers.withMessage('Must be a valid date', validDate)
    },
    month: {
      required:  helpers.withMessage('This field is required', required),
      minLength: helpers.withMessage('Please add leading 0s', minLength(2)),
      minValue:  helpers.withMessage('Must be a valid month', minValue('01')),
      maxValue:  helpers.withMessage('Must be a valid month', maxValue('12')),
      validDate
    },
    year: {
      required:  helpers.withMessage('This field is required', required),
      minLength: helpers.withMessage('Must be a valid year', minLength(4)),
      maxValue:  helpers.withMessage('Must be in the past', maxValue(new Date().getFullYear().toString())),
      validDate
    },
  };
});

const v$ = useVuelidate(rules, formData);

const years = ref(null)
const months = ref(null)
const days = ref(null)

let last = Date.now()

function calculate() {
  last = Date.now()
  let now = new Date()
  let then = new Date(`${formData.year}-${formData.month}-${formData.day}`)
  let yearsTemp = now.getUTCFullYear() - then.getUTCFullYear() 
  let monthsTemp = now.getMonth() - then.getMonth()
  if (then.getDate() > now.getDate()) {
    monthsTemp -= 1
  }
  if (monthsTemp < 0) {
    monthsTemp += 12
    yearsTemp -= 1
  }
  let daysTemp = 0
  while (then.getDate() > now.getDate() && then.getDate() !== 1) {
    then.setDate(then.getDate() + 1)
    daysTemp += 1
    console.log(then.getDate())
  }
  daysTemp += parseInt(now.getDate() - then.getDate())
  years.value = 0
  months.value = 0
  days.value = 0
  let yearInterval = setInterval(() => {
    if(years.value < yearsTemp) {
      years.value += 1
    }
  }, 1000 / yearsTemp)
  let monthInterval = setInterval(() => {
    if(months.value < monthsTemp) {
      months.value += 1
    }
  }, 1000 / monthsTemp)
  let dayInterval = setInterval(() => {
    if(days.value < daysTemp) {
      days.value += 1
    }
  }, 1000 / daysTemp)
  setTimeout(() => {
    clearInterval(yearInterval)
    clearInterval(monthInterval)
    clearInterval(dayInterval)
    years.value = yearsTemp
    months.value = monthsTemp
    days.value = daysTemp
  }, 1500)
}
</script>

<template>
  <main id="app-container"
    class="text-base font-normal font-poppins bg-[--off-white] w-screen h-screen flex items-center justify-center whitespace-nowrap overflow-x-hidden">
    <article class="bg-white p-4 lg:p-6 rounded-xl rounded-br-[25%] flex flex-col gap-6 lg:gap-1 lg:w-[900px] w-fit m-1">
      <form class="flex flex-row gap-2 lg:gap-4">
        <section class="w-16 lg:w-20 flex flex-col shrink-0">
          <label for="day-input" class="text-[0.5rem] font-bold tracking-[0.15rem]" 
          :class="{'text-[--light-red]': v$.day.$error, 'text-[--smokey-grey]': !v$.day.$error}">DAY</label>
          <input id="day-input"
            class="w-full py-1 px-2.5 border border-[--light-grey] rounded-[5px] text-[--off-black] font-bold focus:border-[--purple] focus:outline-none"
            placeholder="DD" maxlength="2" v-model="formData.day"
            :class="{'border-[--light-red]': v$.day.$error}" />
          <p class="text-[14px] italic text-[--light-red] max-w-24 text-wrap leading-normal lg:leading-[1.5rem] lg:max-w-40" v-if="v$.day.$error">{{ v$.day.$errors[0].$message }}</p>
        </section>
        <section class="w-16 lg:w-20 flex flex-col shrink-0">
          <label for="month-input" class="text-[0.5rem] font-bold tracking-[0.15rem]"
          :class="{'text-[--light-red]': v$.month.$error, 'text-[--smokey-grey]': !v$.month.$error}">MONTH</label>
          <input id="month-input"
            class="w-full py-1 px-2.5 border border-[--light-grey] rounded-[5px] text-[--off-black] font-bold focus:border-[--purple] focus:outline-none"
            placeholder="MM" maxlength="2" v-model="formData.month"
            :class="{'border-[--light-red]': v$.month.$error}" />
          <p class="text-[14px] italic text-[--light-red] max-w-24 text-wrap leading-normal lg:leading-[1.5rem] lg:max-w-40" v-if="v$.month.$error">{{ v$.month.$errors[0].$message }}</p>
        </section>
        <section class="w-16 lg:w-20 flex flex-col shrink-0">
          <label for="year-input" class="text-[0.5rem] font-bold tracking-[0.15rem]"
          :class="{'text-[--light-red]': v$.year.$error, 'text-[--smokey-grey]': !v$.year.$error}">YEAR</label>
          <input id="year-input"
            class="w-full py-1 px-2.5 border border-[--light-grey] rounded-[5px] text-[--off-black] font-bold focus:border-[--purple] focus:outline-none"
            placeholder="YYYY" maxlength="4" v-model="formData.year"
            :class="{'border-[--light-red]': v$.year.$error}" />
          <p class="text-[14px] italic text-[--light-red] max-w-24 text-wrap leading-normal lg:leading-[1.5rem] lg:max-w-40" v-if="v$.year.$error">{{ v$.year.$errors[0].$message }}</p>
        </section>
      </form>
      <section class="flex flex-row relative items-center justify-center lg:justify-end">
        <figure class="h-[1px] w-full bg-[--light-grey] absolute" />
        <button :class="{ 'p-3 bg-[--off-black] rounded-full w-fit z-10': true, 'bg-[--purple]': years !== null }"
          @click="() => { 
              v$.$validate();
              if(!v$.$invalid && Date.now() - last > 1500) {
                calculate();
                v$.$reset();
              }
            }">
          <img class="w-4 h-4 lg:w-6 lg:h-6" src="/assets/images/icon-arrow.svg" />
        </button>
      </section>
      <section class="font-extrabold italic mb-6 text-4xl lg:text-5xl">
        <p><em class="text-[--purple]">{{ years === null ? '--' : years }}</em> year{{ years === 1 ? '' : 's' }}</p>
        <p><em class="text-[--purple]">{{ months === null ? '--' : months }}</em> month{{ months === 1 ? '' : 's' }}</p>
        <p><em class="text-[--purple]">{{ days === null ? '--' : days }}</em> day{{ days === 1 ? '' : 's' }}</p>
      </section>
    </article>
  </main>
</template>
