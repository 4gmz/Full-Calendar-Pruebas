<template>
  <div class="demo-app">
    <!-- <FullCalendar
      class="demo-app-calendar"
      ref="fullCalendar"
      defaultView="timeGridWeek"
      :header="header"
      :plugins="calendarPlugins"
      :weekends="calendarWeekends"
      :events="calendarEvents"
      locale="es"
      @dateClick="handleDateClick"
    />-->
    <!-- copia -->
    <FullCalendar
      class="demo-app-calendar"
      ref="fullCalendar"
      :config="config"
      :plugins="calendarPlugins"
      :header="header"
      :events="calendarEvents"
      locale="es"
      @dateClick="handleDateClick"
    />
  </div>
</template>

<script>
import FullCalendar from "@fullcalendar/vue";
import dayGridPlugin from "@fullcalendar/daygrid";
import timeGridPlugin from "@fullcalendar/timegrid";
import interactionPlugin from "@fullcalendar/interaction";

// must manually include stylesheets for each plugin
import "@fullcalendar/core/main.css";
import "@fullcalendar/daygrid/main.css";
import "@fullcalendar/timegrid/main.css";

export default {
  components: {
    FullCalendar // make the <FullCalendar> tag available
  },
  data: function() {
    return {
      config: {},
      calendarPlugins: [dayGridPlugin, timeGridPlugin, interactionPlugin],
      header: {
        left: "prev,next today",
        center: "title",
        right: "dayGridMonth,timeGridWeek,timeGridDay,listWeek"
      },
      defaultView: "timeGridWeek",
      calendarEvents: [
        {
          title: "Event Now",
          start: new Date()
        }
      ]
    };
  },
  created: function() {
    let me = this;
    me.config = {
      calendarWeekends: false,

      eventClick: function(calEvent, jsEvent, view) {
        alert("hola mundo");
      }
      // @dateClick="handleDateClick"
    };
  },
  methods: {
    handleDateClick(arg) {
      console.log("arg.date", arg.date);

      if (confirm("Would you like to add an event to " + arg.dateStr + " ?")) {
        this.calendarEvents.push({
          // add new event data
          title: "New Event",
          start: arg.date,
          allDay: arg.allDay
        });
      }
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
