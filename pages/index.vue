<template>
  <v-layout>
    <v-flex text-xs-center>
      <v-card v-for="(event, i) in events" :key="i" elevation="5" style="margin-bottom: 1em">
        <v-card-title style="font-weight:bold">{{ event.description }}</v-card-title>
        <v-chip v-if="event.speaker!=null"><v-icon>person</v-icon>{{ event.speaker }}</v-chip>
        <v-chip color="green">{{ event.location }}</v-chip>
      </v-card>
    </v-flex>
  </v-layout>
</template>

<script>
export default {
  name: 'Agenda',
  data() {
    return {
      events: [
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
        { speaker: null, description: 'Abendveranstaltung - Überraschung ;-)', location: 'wird noch bekanntgegeben', startTime: new Date('2019-06-20T17:00:00+02:00'), endTime: new Date('2019-06-20T20:00:00+02:00') }
      ]
    }
  }
}
</script>
