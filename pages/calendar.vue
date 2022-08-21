<template>
    <div>
        <Calendar ref="baseCalendar" v-on:getEvents="getEvents" v-on:selectedEventOpen="selectedEventOpen"
            v-on:endDrag="endDrag" v-on:createEvent="createEvent">
            <template v-slot:selectedevent>
                <p>{{ selectedTime }}</p>
                <v-form>
                    <v-text-field v-model="selectedEventName"></v-text-field>
                    
                </v-form>
            </template>
            <template v-slot:selected-event-actions>
                <v-btn @click="modifyTitle">Update</v-btn>
                <v-btn @click="deleteConfirmOpen = true">Delete</v-btn>
            </template>
        </Calendar>
        <v-dialog width="500" v-model="deleteConfirmOpen">
            <v-card color="grey lighten-4" min-width="350px" flat>
                <v-card-title class="text-h5 grey lighten-2">
                    削除確認 [{{ selectedEventName }}]
                </v-card-title>
                <v-card-text> 削除してもよろしいですか？</v-card-text>
                <v-card-actions>
                    <v-spacer></v-spacer>
                    <v-btn color="primary blue" text @click="deleteEvent"> はい </v-btn>
                    <v-btn color="primary" text @click="deleteConfirmOpen = false">
                        キャンセル
                    </v-btn>
                </v-card-actions>
            </v-card>
        </v-dialog>
    </div>
</template>

<script lang="ts">
//@ts-ignore
import { Component, Vue } from "vue-property-decorator";
import Calendar from "../components/Calendar.vue";
//@ts-ignore
import { gql } from "nuxt-graphql-request";
//@ts-ignore
import dateFormat from "dateformat";

interface EventObject {
    id: string | null;
    start: number;
    end: number;
    color: string | null;
    name: string | null;
    timed: boolean | null;
}

@Component({
    components: { Calendar },
})
export default class CalendarPage extends Vue {
    selectedEventName: string | null = "";
    selectedEvent: EventObject | null = null;
    deleteConfirmOpen: boolean = false;

    $refs!: {
        baseCalendar: any
    }

    get selectedTime(){

        if(this.selectedEvent == null)
        {
            return ""
        }
        const st:string = dateFormat(new Date(this.selectedEvent.start),"yyyy/mm/dd HH:MM")
        const ed:string = dateFormat(new Date(this.selectedEvent.end),"yyyy/mm/dd HH:MM")
        return `${st} - ${ed}`
    }

    deleteEvent() {
        (async () => {
            if (this.selectedEvent == null) return;

            const event: EventObject = this.selectedEvent;
            const nmt: string = gql`
                    mutation del($id: ID!) {
                    deleteSchedule(input: { id: $id })
                    }
                `;
            try {

                //@ts-ignore
                await this.$nuxt.$graphql.default.request(nmt, {
                    id: event.id,
                    summary: event.name,
                });

                const events: EventObject[] = this.$refs.baseCalendar.events
                const key = events.indexOf(this.selectedEvent)
                if (key !== -1) {
                    events.splice(key, 1)
                }



            } catch (error) {
                alert(error)
            }
        })();
        this.deleteConfirmOpen = false;
    }

    selectedEventOpen(event: EventObject) {
        this.selectedEventName = event.name;
        this.selectedEvent = event;
    }

    modifyTitle() {
        (async () => {
            if (this.selectedEvent == null) return;
            const event: EventObject = this.selectedEvent;
            event.name = this.selectedEventName;
            const nmt: string = gql`
        mutation mst($id: ID!, $summary: String!) {
          modifyScheduleTitle(input: { id: $id, summary: $summary })
        }
      `;

            try {
                //@ts-ignore
                await this.$nuxt.$graphql.default.request(nmt, {
                    id: event.id,
                    summary: event.name,
                });
                //@ts-ignore
                this.$refs.baseCalendar.selectedOpen = false;
            } catch (error) {
                alert("更新に失敗しました");
            }
        })();
    }

    createEvent(event: EventObject) {
        (async () => {
            const nmt: string = gql`
        mutation cs($start: Time!, $end: Time!, $summary: String!) {
          createSchedule(
            input: { start: $start, end: $end, summary: $summary }
          ) {
            id
          }
        }
      `;

            //@ts-ignore
            const rs = await this.$nuxt.$graphql.default.request(nmt, {
                start: dateFormat(new Date(event.start), "isoUtcDateTime"),
                end: dateFormat(new Date(event.end), "isoUtcDateTime"),
                summary: event.name,
            });
            event.id = rs.createSchedule.id;
        })();
    }

    endDrag(event: EventObject | null) {
        if (event == null || event.id == undefined) return;

        (async () => {
            const mt: string = gql`
        mutation ms($id: ID!, $start: Time!, $end: Time!) {
          modifySchedule(input: { id: $id, start: $start, end: $end })
        }
      `;
            //@ts-ignore
            const rs = await this.$nuxt.$graphql.default.request(mt, {
                id: event.id,
                start: dateFormat(new Date(event.start), "isoUtcDateTime"),
                end: dateFormat(new Date(event.end), "isoUtcDateTime"),
            });
        })();
    }

    /**
     * イベント情報の読み込み
     * 
     * @param start 
     * @param end 
     * @param events 
     */
    async getEvents(start: any, end: any, events: EventObject[]) {

        const query: string = gql`
            query schedules($start: Time, $end:Time) {
                schedules (start:$start, end:$end){
                id
                summary
                start
                end
                }
            }
            `;

        let st = new Date(start.date)
        st = new Date(st.getTime()-24*60*60*1000)
        
        let ed = new Date(end.date)
        ed = new Date(ed.getTime()+24*60*60*1000)

        //@ts-ignore        
        const sc = await this.$nuxt.$graphql.default.request(query, {
            start: dateFormat(st, "isoUtcDateTime"),
            end: dateFormat(ed, "isoUtcDateTime")
        });

        sc.schedules.forEach(
            (s: { id: string; start: string; end: string; summary: string }) => {

                events.push({
                    id: s.id,
                    start: Date.parse(s.start),
                    end: Date.parse(s.end),
                    name: s.summary,
                    timed: true,
                    color: "#000000",
                });
            }
        );
    }
}
</script>
