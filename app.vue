<template>
  <div id="app-container" class="text-base font-normal font-poppins bg-[--off-white] w-screen h-screen flex items-center justify-center">
    <div class="bg-white p-10 rounded-md rounded-br-[25%] flex flex-col gap-1">
      <div class="flex flex-row gap-10">
        <div class="flex flex-col gap-1">
          <label>DAY</label>
          <input class="w-20" />
        </div>
        <div class="flex flex-col gap-1">
          <label>MONTH</label>
          <input class="w-20" />
        </div>
        <div class="flex flex-col gap-1">
          <label>YEAR</label>
          <input class="w-20" />
        </div>
      </div>
      <hr>
      <div class="font-extrabold italic text-[32px]"><span class="text-[--purple]">--</span> years</div>
      <div class="font-extrabold italic text-[32px]"><span class="text-[--purple]">--</span> months</div>
      <div class="font-extrabold italic text-[32px]"><span class="text-[--purple]">--</span> days</div>
    </div>
  </div>
</template>
