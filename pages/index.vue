<template>
  <div class="page">
    <div class="box">
      <input type="text"  @keypress="setQuery" v-model="query" />

      {{list}}
    </div>
  </div>
</template>


<script setup type="ts">
import { ref, reactive } from "#imports";

const API_KEY = "2fa73590fd8b5a4c6e68098ad5625395";

const query = ref('')

let list = reactive({})

function setQuery(evt) {
  if (evt.keyCode == 13) {
    api(query.value);
  }
}

function api(queryValue) {
  fetch(
      `https://api.openweathermap.org/data/2.5/weather?q=${queryValue}&units=metric&APPID=${API_KEY}`
  )
      .then((res) => res.json())
      .then(displayResults);
}

function displayResults(weather) {
  console.log(weather);
  list.value = weather
}

const days = [
  "Sunday",
  "Monday",
  "Tuesday",
  "Wednesday",
  "Thursday",
  "Friday",
  "Saturday",
];
const months = [
  "Jan",
  "Feb",
  "Mar",
  "Apr",
  "May",
  "Jun",
  "Jul",
  "Aug",
  "Sep",
  "Oct",
  "Nov",
  "Dec",
];

const vaqt = reactive({
  timee: "",
  data: "",
});

setInterval(() => {
  const time = new Date();
  const month = time.getMonth();
  const date = time.getDate();
  const day = time.getDay();
  const hour = time.getHours();
  const hoursIn12HrFormat = hour >= 13 ? hour % 12 : hour;
  const minutes = time.getMinutes();
  const ampm = hour >= 12 ? "PM" : "AM";
  vaqt.timee = `${
    hoursIn12HrFormat < 10 ? "0" + hoursIn12HrFormat : hoursIn12HrFormat
  }:${minutes < 10 ? "0" + minutes : minutes} ${ampm}`;

  vaqt.data = days[day] + ", " + date + " " + months[month];
}, 1000);
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
.page {
  width: 100%;
  height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
}

.box {
  padding: 20px;
  width: 500px;
  border: 1px solid #000;
}
</style>