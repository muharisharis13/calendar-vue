<template>
  <div class="home">
    <button @click="addNewEvent">add event</button>
    <FullCalendar :options="calendarOptions" />
  </div>
</template>

<script>
import FullCalendar from "@fullcalendar/vue";
import dayGridPlugin from "@fullcalendar/daygrid";
import interactionPlugin from "@fullcalendar/interaction";
export default {
  name: "HomeView",
  components: {
    FullCalendar, // make the <FullCalendar> tag available
  },
  data() {
    return {
      calendarOptions: {
        plugins: [dayGridPlugin, interactionPlugin],
        initialView: "dayGridMonth",
        selectable: true,
        editable: true,
        events: [
          {
            // this object will be "parsed" into an Event Object
            title: "The Title", // a property!
            start: "2023-09-01", // a property!
            end: "2023-09-03", // a property! ** see important note below about 'end' **
          },
        ],
        dateClick: this.remove,
        eventClick: this.handleEvent,
      },
    };
  },
  methods: {
    addNewEvent() {
      let date = window.prompt("add new events YYYY-MM-DD");
      let nameEvent = window.prompt("add name event");
      this.calendarOptions.events = [
        ...this.calendarOptions.events,
        { title: nameEvent, date: date },
      ];
    },
    handleEvent(props) {
      let event = props.event;
      let result = window.confirm(
        "Are you edit Event or Delete? If Delete please OK"
      );
      console.log([result, props]);

      if (result) {
        this.calendarOptions.events = this.calendarOptions.events.filter(
          (filter) => filter.title !== event.title
        );
      } else {
        let nameEvent = window.prompt("change name Event", event.title);

        let findName = this.calendarOptions.events.findIndex(
          (obj) => obj.title == event.title
        );

        this.calendarOptions.events[findName].title = nameEvent
          ? nameEvent
          : event.title;
      }
    },
  },
};
</script>
