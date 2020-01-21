<template>
  <div class="demo-app">
    <FullCalendar
      class="demo-app-calendar"
      ref="fullCalendar"
      defaultView="dayGridMonth"
      :header="{
        left: 'prev,next today',
        center: 'title',
        right: 'dayGridMonth,timeGridWeek,timeGridDay,listWeek'
      }"
      :plugins="calendarPlugins"
      selectable="selectable"
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
      calendarEvents: [{ title: "Event Now", start: new Date() }]
    };
  },
  created: function() {
    // let me = this;
    // me.config = {
    //   calendarWeekends: false,
    //   allDaySlot: false,
    //   editable: false,
    //   selectable: true,
    //   selectHelper: true,
    //   eventClick: function(calEvent, jsEvent, view) {
    //     alert("hola mundo");
    //   }
    //   // @dateClick="handleDateClick"
    // };
  },
  methods: {
    handleDateClick(arg) {
      // console.log("arg.date", arg);
      console.log("el star:", moment(arg.dateStr).add(30, "minutes"));
      if (confirm("Would you like to add an event to " + arg.dateStr + " ?")) {
        this.calendarEvents.push({
          // add new event data
          title: "New Event",
          start: arg.date,
          end: arg.date,
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
