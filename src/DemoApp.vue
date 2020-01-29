<template>
  <div class="demo-app">
    <div class="row my-4">
      <button class="btn btn-danger my-2" @click="handleButtonClick">Boton de conole</button>
    </div>
    <FullCalendar
      class="demo-app-calendar"
      ref="fullCalendar"
      locale="es"
      defaultView="timeGridWeek"
      :header="{ left: 'prev,next today',center: 'title',right:'dayGridMonth, timeGridWeek, timeGridDay, listWeek' }"
      :button-text="{ today:'Hoy', month:'Mes', week: 'Semana', day:'Día', list: 'Lista' } "
      :views="views"
      :plugins="calendarPlugins"
      :selectable="true"
      :allDaySlot="true"
      :weekends="true"
      :firstDay="1"
      allDayText="todo el día"
      min-time="08:00:00"
      max-time="18:00:00"
      :events="calendarEvents"
      @select="handleSelect"
      @dateClick="handleDateClick"
      @eventClick="handleClick"
    />
  </div>
</template>

<script>
import FullCalendar from "@fullcalendar/vue";
import dayGridPlugin from "@fullcalendar/daygrid";
import timeGridPlugin from "@fullcalendar/timegrid";
import interactionPlugin from "@fullcalendar/interaction";
import { Calendar } from "@fullcalendar/core";
import listPlugin from "@fullcalendar/list";

import moment from "moment";

// must manually include stylesheets for each plugin
import "@fullcalendar/core/main.css";
import "@fullcalendar/daygrid/main.css";
import "@fullcalendar/timegrid/main.css";
import "@fullcalendar/list/main.css";

export default {
  components: {
    FullCalendar
  },
  data: function() {
    return {
      calendarPlugins: [
        dayGridPlugin,
        timeGridPlugin,
        interactionPlugin,
        listPlugin
      ],
      // selectable: true,
      // calendarWeekends: true,
      views: { dayGrid: { eventLimit: 4 } },
      calendarEvents: [{ title: "Event Now", start: new Date() }]
    };
  },
  methods: {
    handleDateClick(arg) {
      // console.log("arg.date", arg);
      let inicio = moment(arg.date).format("YYYY-MM-DD H:mm:ss a");
      let fin = moment(arg.date)
        .add(45, "minutes")
        .format("YYYY-MM-DD H:mm:ss a");
      console.log(
        "hora fin",
        moment(fin)
          .add(45, "minutes")
          .format("YYYY-MM-DD H:mm:ss a"),
        "hora de inicio:",
        moment(arg.date).format("YYYY-MM-DD H:mm:ss a")
      );
      if (confirm("Would you like to add an event to " + arg.dateStr + " ?")) {
        this.calendarEvents.push({
          // add new event data
          title: "New Event",
          start: moment(arg.date).format("YYYY-MM-DDTHH:mm:ssZ"),
          end: moment(arg.date)
            .add(45, "minutes")
            .format("YYYY-MM-DDTHH:mm:ssZ"),
          id: 1,
          color: "blue",
          groupId: 1
        });
      }
    },
    handleSelect(e) {
      // console.log("el select event ", e);
    },
    handleClick(info) {
      var eventObj = info.event;
      console.log("info: ", eventObj.title);
    },
    handleDate(semana) {
      // permite moverse entre semanas del calendario. semana
      let fecha = moment(moment().week(semana))
        .startOf("week")
        .format("YYYY-MM-DD HH:mm:ss");
      this.$refs.calendar.getApi().gotoDate(fecha);
    },
    handleButtonClick() {
      let calendarApi = this.$refs.fullCalendar;
      console.log("calendar", calendarApi);
    }
  }
};
</script>

<style>
.demo-app {
  font-family: Arial, Helvetica Neue, Helvetica, sans-serif;
  font-size: 14px;
}

.demo-app-top {
  margin: 0 0 3em;
}

.demo-app-calendar {
  margin: 0 auto;
  max-width: 900px;
}
</style>
