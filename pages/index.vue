<template>
  <v-layout v-touch="{ left() { nextDay() }, right() { previousDay() } }">
    <v-flex text-xs-center>
      <div class="weekday-info">
        {{ currentWeekdayName }}
        <v-icon @click="previousDay()">
          arrow_left
        </v-icon>
        <v-icon @click="nextDay()">
          arrow_right
        </v-icon>
      </div>
      <div v-for="(event, i) in events" :key="i">
        <div v-if="!event.sameTime" class="time-info">
          {{ event.startTimeString }} Uhr
        </div>
        <v-card elevation="5" style="margin-bottom: 1em">
          <v-card-title style="font-weight:bold">
            {{ event.description }}
          </v-card-title>
          <v-chip v-if="event.speaker!=null">
            <v-icon>person</v-icon>
            {{ event.speaker }}
          </v-chip>
          <v-chip color="green">
            {{ event.location }}
          </v-chip>
        </v-card>
      </div>
    </v-flex>
  </v-layout>
</template>

<style>
.dev_border {
  border: #7f7f7f 1px dashed;
}
.weekday-info {
  text-align: left;
  font-size: 150%;
  font-weight: bold;
  padding: 0.1em 0.75em;
}
.time-info {
  text-align: left;
  font-size: 150%;
  padding: 0.25em 0.75em;
}
</style>

<script>
export default {
  name: 'Agenda',
  data() {
    return {
      selectedDate: null,
      rawEvents: [
        { speaker: null, description: 'Wanderung', location: 'Wald', startTime: new Date('2019-06-19T18:00:00+02:00'), endTime: new Date('2019-06-19T21:00:00+02:00') },
        { speaker: null, description: 'Party', location: 'Bar', startTime: new Date('2019-06-19T18:00:00+02:00'), endTime: null },

        { speaker: null, description: 'Frühstück', location: 'Buffet', startTime: new Date('2019-06-20T07:30:00+02:00'), endTime: new Date('2019-06-20T09:15:00+02:00') },
        { speaker: 'Anton Angermeier', description: 'Eröffnungsrede', location: 'Plenum', startTime: new Date('2019-06-20T09:30:00+02:00'), endTime: new Date('2019-06-20T09:45:00+02:00') },
        { speaker: 'Berta Baumann', description: 'Historischer Rückblick', location: 'Plenum', startTime: new Date('2019-06-20T09:45:00+02:00'), endTime: new Date('2019-06-20T10:45:00+02:00') },
        { speaker: 'Chris Carlsson', description: 'Aktuelle Entwicklungen', location: 'Plenum', startTime: new Date('2019-06-20T10:45:00+02:00'), endTime: new Date('2019-06-20T11:45:00+02:00') },
        { speaker: null, description: 'Mittagspause', location: 'Buffet', startTime: new Date('2019-06-20T11:45:00+02:00'), endTime: new Date('2019-06-20T13:00:00+02:00') },
        { speaker: 'Doris Decker', description: 'Erfahrungen aus dem Sport', location: 'Raum 4', startTime: new Date('2019-06-20T13:00:00+02:00'), endTime: new Date('2019-06-20T14:00:00+02:00') },
        { speaker: 'Emil Eismann', description: 'Erfahrungen aus der Wirtschaft', location: 'Raum 3', startTime: new Date('2019-06-20T13:00:00+02:00'), endTime: new Date('2019-06-20T14:00:00+02:00') },
        { speaker: 'Franziska Fungo', description: 'Erfahrungen aus der Natur', location: 'Raum 2', startTime: new Date('2019-06-20T14:15:00+02:00'), endTime: new Date('2019-06-20T15:15:00+02:00') },
        { speaker: 'Gerd Grundmann', description: 'Erfahrungen vom Bau', location: 'Raum 3', startTime: new Date('2019-06-20T14:15:00+02:00'), endTime: new Date('2019-06-20T15:15:00+02:00') },
        { speaker: null, description: 'Kaffeepause', location: 'Café', startTime: new Date('2019-06-20T15:15:00+02:00'), endTime: new Date('2019-06-20T16:00:00+02:00') },
        { speaker: null, description: 'Abendveranstaltung - Überraschung ;-)', location: 'wird noch bekanntgegeben', startTime: new Date('2019-06-20T17:00:00+02:00'), endTime: new Date('2019-06-20T20:00:00+02:00') },

        { speaker: null, description: 'Frühstück', location: 'Buffet', startTime: new Date('2019-06-21T07:30:00+02:00'), endTime: new Date('2019-06-21T09:15:00+02:00') },
        { speaker: 'Hannelore Hempel', description: 'Die Zukunft im Blick', location: 'Plenum', startTime: new Date('2019-06-21T09:30:00+02:00'), endTime: new Date('2019-06-21T10:30:00+02:00') },
        { speaker: 'Ingo Insauer', description: 'Erfahrungen aus der Wissenschaft', location: 'Raum 2', startTime: new Date('2019-06-21T10:45:00+02:00'), endTime: new Date('2019-06-21T11:45:00+02:00') },
        { speaker: 'Jana Jäger', description: 'Erfahrungen aus der Musik', location: 'Raum 4', startTime: new Date('2019-06-21T10:45:00+02:00'), endTime: new Date('2019-06-21T11:45:00+02:00') },
        { speaker: 'Karl Klammer', description: 'Erfahrungen aus der Kunst', location: 'Raum 3', startTime: new Date('2019-06-21T10:45:00+02:00'), endTime: new Date('2019-06-21T11:45:00+02:00') },
        { speaker: null, description: 'Mittagspause', location: 'Buffet', startTime: new Date('2019-06-21T11:45:00+02:00'), endTime: new Date('2019-06-21T13:00:00+02:00') },
        { speaker: 'Laura Lindner', description: 'Das beste Best-Of aller Zeiten', location: 'Plenum', startTime: new Date('2019-06-21T13:00:00+02:00'), endTime: new Date('2019-06-21T14:00:00+02:00') },
        { speaker: 'Anton Angermeier', description: 'Zusammenfassung und Abschluss', location: 'Plenum', startTime: new Date('2019-06-21T14:00:00+02:00'), endTime: new Date('2019-06-21T14:30:00+02:00') }
      ]
    }
  },
  computed: {
    events() {
      const result = []
      let lastEvent = null
      this.rawEvents.filter((event) => {
        return this.selectedDate === null || event.startTime.getDay() === this.selectedDate.getDay()
      }).sort((a, b) => {
        return a.startTime.getTime() - b.startTime.getTime()
      }).forEach((event) => {
        const newEvent = { ...event }
        newEvent.sameTime = (lastEvent && event.startTime.getTime() === lastEvent.startTime.getTime())
        newEvent.startTimeString = event.startTime.getHours() + ':' + (event.startTime.getMinutes() < 10 ? '0' : '') + event.startTime.getMinutes()
        result.push(newEvent)
        lastEvent = event
      })
      return result
    },
    firstDate() {
      return new Date(this.rawEvents.reduce((total, current) => {
        return Math.min(total === null ? current.startTime.getTime() : total, current.startTime.getTime())
      }, null))
    },
    lastDate() {
      return new Date(this.rawEvents.reduce((total, current) => {
        return Math.max(total === null ? current.startTime.getTime() : total, current.startTime.getTime())
      }, null))
    },
    currentWeekdayName() {
      return this.getWeekdayName(this.selectedDate.getDay())
    }
  },
  created() {
    this.selectDate(this.firstDate)
  },
  methods: {
    getWeekdayName(weekdayNumber) {
      return ['Sonntag', 'Montag', 'Dienstag', 'Mittwoch', 'Donnerstag', 'Freitag', 'Samstag'][weekdayNumber]
    },
    selectDate(date) {
      this.selectedDate = date
    },
    date2int(date) {
      return date.getFullYear() * 10000 + date.getMonth() * 100 + date.getDate()
    },
    previousDay() {
      if (this.date2int(this.selectedDate) > this.date2int(this.firstDate)) {
        this.selectDate(new Date(this.selectedDate.getTime() - 86400000))
      }
    },
    nextDay() {
      if (this.date2int(this.selectedDate) < this.date2int(this.lastDate)) {
        this.selectDate(new Date(this.selectedDate.getTime() + 86400000))
      }
    }
  }
}
</script>
