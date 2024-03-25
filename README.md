# Frontend Mentor - Age calculator app solution

This is a solution to the [Age calculator app challenge on Frontend Mentor](https://www.frontendmentor.io/challenges/age-calculator-app-dF9DFFpj-Q).

## Table of contents

- [Overview](#overview)
  - [The challenge](#the-challenge)
  - [Screenshot](#screenshot)
  - [Links](#links)
- [My process](#my-process)
  - [Built with](#built-with)
  - [What I learned](#what-i-learned)
  - [Continued development](#continued-development)
  - [Useful resources](#useful-resources)
- [Author](#author)

## Overview

### The challenge

Users are able to:

- ✔️️ View an age in years, months, and days after submitting a valid date through the form
- ✔️ Receive validation errors if:
  - ✔️ Any field is empty when the form is submitted
  - ✔️ The day number is not between 1-31
  - ✔️ The month number is not between 1-12
  - ✔️ The year is in the future
  - ✔️ The date is invalid e.g. 31/04/1991 (there are 30 days in April)
- ✔️ View the optimal layout for the interface depending on their device's screen size
- ✔️ See hover and focus states for all interactive elements on the page
- ✔️ **Bonus**: See the age numbers animate to their final number when the form is submitted

### Screenshot

![Screenshot of the website](/public/screenshot.png)

### Links

- [Solution](https://www.frontendmentor.io/challenges/age-calculator-app-dF9DFFpj-Q)
- [Live Site](https://fm-age-calculator-app-blond.vercel.app/)

## My process

### Built with

- Semantic HTML5 markup
- Mobile-first workflow
- Static fonts
- [Vue](https://vuejs.org/) - JS library
- [Nuxt.js](https://nuxt.com/) - Vue framework
- [Tailwind](https://tailwindcss.com/) - For styles
- [Vuelidate](https://vuelidate-next.netlify.app/) - For form validation

### What I learned

Form validation using Vuelidate:

```js
day: {
  required:  helpers.withMessage('This field is required', required),
  minLength: helpers.withMessage('Please add leading 0s', minLength(2)),
  minValue:  helpers.withMessage('Must be a valid day', minValue('01')),
  maxValue:  helpers.withMessage('Must be a valid day', maxValue('31')),
  validDate: helpers.withMessage('Must be a valid date', validDate)
},
```

The animation written in Javascript. The values shown increase until they reach their correct value in exactly one second.

```js
let yearInterval = setInterval(() => {
  if(years.value < yearsTemp) {   // yearsTemp stores the final number of years
    years.value += 1
  }
}, 1000 / yearsTemp)
let monthInterval = setInterval(() => {
  if(months.value < monthsTemp) { // monthsTemp stores the final number of months
    months.value += 1
  }
}, 1000 / monthsTemp)
let dayInterval = setInterval(() => {
  if(days.value < daysTemp) {     // daysTemp stores the final number of days
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
```

### Continued development

- I want to learn how to use the built-in API endpoints for form validation as it will help me build real-world apps.
- I will learn how to apply global styles using Tailwind and I will also learn more about how the Tailwind config file can be used.
- I will look into Next.js with React as another popular framework option.

### Useful resources

- [Vue docs](https://vuejs.org/guide/introduction)
- [Cool Tailwind video](https://www.youtube.com/watch?v=pfaSUYaSgRo)
- The Mozilla references for [HTML](https://developer.mozilla.org/en-US/docs/Web/HTML) and [CSS](https://developer.mozilla.org/en-US/docs/Web/CSS) are always useful.

## Author

- LeetCode - [@SerbanUntu](https://leetcode.com/SerbanUntu/)
- Github - [@SerbanUntu](https://github.com/SerbanUntu)
- Frontend Mentor - [@SerbanUntu](https://www.frontendmentor.io/profile/SerbanUntu)