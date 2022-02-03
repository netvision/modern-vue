<script setup name="IndexPage">
import axios from 'axios'
import { GoogleMap, Marker } from 'vue3-google-map'
//28.241271107538967, 75.65319151008205
const center = { lat: 28.241271107538967, lng: 75.65319151008205 }

const logs = ref([])

const info = ref({})

const showInfo = (log) => {
  console.log(log.info)
  info.value = log.info
}

onMounted(async () => {
  let res = await axios
    .get('http://rkjdss.dalmiatrusts.in/rjdssapi/web/staff-logs/daily?d=2022-02-01')
    .then((r) => r.data)
  res.forEach((r) => {
    let log = {
      location: { lat: +r.latitude, lng: +r.longitude },
      label: r.user,
      title: r.user,
      info: r,
    }
    logs.value.push(log)
  })
  console.log(res)
})
</script>

<template>
  <div class="relative">
    <h2>Admin Dashboard</h2>
    <GoogleMap
      api-key="AIzaSyDG9I2B0BJsIx9gtd7Hw9_9_0QA4xtBHnw"
      style="width: 100%; height: 500px"
      :center="center"
      :zoom="10"
    >
      <Marker
        v-for="(log, i) in logs"
        :key="i"
        :options="{ position: log.location, label: log.label, title: log.title }"
        @click="showInfo(log)"
      />
    </GoogleMap>
    <div class="absolute top-12 right-4 w-lg shadow-md shadow-gray-500 bg-blue-500/50">
      <h2 class="font-serif text-2xl font-medium text-left">
        {{ info.user }} @ {{ info.date_time }}
      </h2>
      <hr />
      <h2 class="text-xl font-medium text-left">
        {{ info.contacted_person }}
      </h2>
      <p class="text-lg font-medium text-left">
        <span>{{ info.discussion }}</span
        ><br />
        {{ info.other_info }}
      </p>
    </div>
  </div>
</template>

<route lang="yaml">
meta:
  layout: home
</route>
