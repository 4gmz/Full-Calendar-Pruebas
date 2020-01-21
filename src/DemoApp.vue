<template>
  <div class="demo-app">
    <FullCalendar
      class="demo-app-calendar"
      ref="fullCalendar"
      defaultView="timeGridWeek"
      :header="{
        left: 'prev,next today',
        center: 'title',
        right: 'dayGridMonth,timeGridWeek,timeGridDay,listWeek'
      }"
      :views="views"
      :plugins="calendarPlugins"
      selectable="selectable"
      :allDaySlot="false"
      :weekends="calendarWeekends"
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
import moment from "moment";

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
      calendarPlugins: [dayGridPlugin, timeGridPlugin, interactionPlugin],
      selectable: true,
      calendarWeekends: false,
      views: {
        dayGrid: {
          eventLimit: 4 // adjust to 6 only for timeGridWeek/timeGridDay
        }
      },
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
          start: arg.date,
          end: moment(arg.date).add(45, "minutes"),
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
