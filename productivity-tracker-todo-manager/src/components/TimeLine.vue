<template>
  <v-calendar
    ref="calendar"
    v-model="value"
    :weekdays="weekday"
    type="day"
    :events="events"

    :event-overlap-mode="mode"
    :event-overlap-threshold="30"
    :event-color="getEventColor"
    @change="getEvents"
  ></v-calendar>
</template>

<script>
export default {
  data: () => ({
    type: "month",
    mode: "column",
    modes: ["stack", "column"],
    weekday: [0, 1, 2, 3, 4, 5, 6],
    weekdays: [{ text: "Mon - Fri", value: [1, 2, 3, 4, 5] }],
    value: "",
    events: [],
    colors: [
      "blue",
      "indigo",
      "deep-purple",
      "cyan",
      "green",
      "orange",
      "grey darken-1",
    ],
    names: [
      "Meeting",
      "Teams",
      "Figma",
      "Zoom",
      "Youtube",
      "Facebook",
      "Instagram",
      "Adobe",
    ],
  }),
  methods: {
    getEvents({ start, end }) {
      const events = [];

      const min = new Date(`${start.date}T08:00:00`);
      const max = new Date(`${end.date}T22:00:00`); // Adjusted to 10 pm
      const days = (max.getTime() - min.getTime()) / 86400000;
      const eventCount = this.rnd(days, days + 20);

      for (let i = 0; i < eventCount; i++) {
        const allDay = this.rnd(0, 3) === 0;
        const firstTimestamp = this.rnd(min.getTime(), max.getTime());
        const first = new Date(firstTimestamp - (firstTimestamp % 900000));
        const secondTimestamp = this.rnd(2, allDay ? 288 : 8) * 900000;
        const second = new Date(first.getTime() + secondTimestamp);

        events.push({
          name: this.names[this.rnd(0, this.names.length - 1)],
          start: first,
          end: second,
          color: this.colors[this.rnd(0, this.colors.length - 1)],
          timed: !allDay,
        });
      }

      this.events = events;
    },
    getEventColor(event) {
      return event.color;
    },
    rnd(a, b) {
      return Math.floor((b - a + 1) * Math.random()) + a;
    },
  },
};
</script>
