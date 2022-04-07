<script setup>
import { reactive } from "vue"; // => est la capacité pour une variable (array, string, number, object, etc) de se mettre à jour lorsque sa valeur ou toute autre variable à laquelle elle fait référence est modifiée après la déclaration
import "@fullcalendar/core/vdom"; // solves problem with Vite
import FullCalendar from "@fullcalendar/vue3";
import dayGridPlugin from "@fullcalendar/daygrid";
import timeGridPlugin from "@fullcalendar/timegrid";
import listPlugin from "@fullcalendar/list";
import interactionPlugin from "@fullcalendar/interaction";
//import useEvents from '../composables/useEvents.js'

//const id = ref(10);

//const ( getEvents ) = useEvents[]

const options = reactive({
  plugins: [dayGridPlugin, timeGridPlugin, listPlugin, interactionPlugin],
  initialView: "dayGridMonth",
  headerToolbar: {
    left: "prev,next, today",
    center: "titre",
    right: "dayGridMonth, dayGridWeek, listDay",
  },
  editable: true,
  selectable: true,
  weekends: true,
  select: (arg) => { //argument est passé à la directive
    id.value = id.value + 1;

    const cal = arg.view.calendar;
    cal.unselect();
    cal.addEvent({
      id: `${id.value}`,
      title: `New event ${id.value}`,
      start: arg.start,
      end: arg.end,
      allDay: true,
    });
  },
  eventClick: (arg) => {
    if (arg.event) {
      arg.event.remove();
    }
  },
  events: [],
  eventAdd: (arg) => {
    createEvent({
      id: arg.event.id,
      title: arg.event.title,
      start: arg.event.start,
      end: arg.event.end,
      allDay: arg.event.allDay,
    });
  },
  eventChange: (arg) => {
    updateEvent({
      id: arg.event.id,
      title: arg.event.start,
      start: arg.event.start,
      end: arg.event.end,
      allDay: arg.event.allDay,
    });
  },
  eventRemove: (arg) => {
    deleteEvent(arg.event.id);
  },
});

// options.events = getEvents.value;
// watch(getEvents, () => {
//   options.events = getEvents.value;
// });
</script>

<template>
  <FullCalendar v-bind:options="options" />
</template>
