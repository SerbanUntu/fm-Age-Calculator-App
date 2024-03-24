<script setup>
const inputDay = ref(null)
const inputMonth = ref(null)
const inputYear = ref(null)

const years = ref(null)
const months = ref(null)
const days = ref(null)

function calculate() {
  let now = new Date()
  let then = new Date(`${inputYear.value}-${inputMonth.value}-${inputDay.value}`)
  years.value = now.getUTCFullYear() - then.getUTCFullYear()
  months.value = now.getMonth() - then.getMonth()
  if(then.getDate() > now.getDate()) {
    months.value -= 1
  }
  if(months.value < 0) {
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
    <div class="bg-white p-10 rounded-xl rounded-br-[25 %] flex flex-col gap-1 max-w-[900px] w-[75%]">
      <div class="flex flex-row gap-5">
        <div class="flex flex-col">
          <label class="text-[0.5rem] text-[--smokey-grey] font-bold tracking-[0.15rem]">DAY</label>
          <input
            class="w-20 py-1 px-2.5 border border-[--light-grey] rounded-[5px] text-[--off-black] font-bold focus:border-[--purple] focus:outline-none"
            placeholder="DD" maxlength="2" required pattern="\d{2}" v-model="inputDay" />
        </div>
        <div class="flex flex-col">
          <label class="text-[0.5rem] text-[--smokey-grey] font-bold tracking-[0.15rem]">MONTH</label>
          <input
            class="w-20 py-1 px-2.5 border border-[--light-grey] rounded-[5px] text-[--off-black] font-bold focus:border-[--purple] focus:outline-none"
            placeholder="MM" maxlength="2" required pattern="\d{2}" v-model="inputMonth" />
        </div>
        <div class="flex flex-col">
          <label class="text-[0.5rem] text-[--smokey-grey] font-bold tracking-[0.15rem]">YEAR</label>
          <input
            class="w-20 py-1 px-2.5 border border-[--light-grey] rounded-[5px] text-[--off-black] font-bold focus:border-[--purple] focus:outline-none"
            placeholder="YYYY" maxlength="4" required pattern="\d{4}" v-model="inputYear" />
        </div>
      </div>
      <div class="flex flex-row relative items-center">
        <div class="h-[1px] w-full bg-[--light-grey]" />
        <button :class="{'p-3 bg-[--off-black] rounded-full w-fit ml-auto' : true, 'bg-[--purple]' : years !== null}" @click="calculate()">
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
