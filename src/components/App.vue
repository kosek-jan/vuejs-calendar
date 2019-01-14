<template>
    <div>
        <div id="day-bar">
            <div>Mon</div>
            <div>Tue</div>
            <div>Wed</div>
            <div>Thu</div>
            <div>Fri</div>
            <div>Sat</div>
            <div>Sun</div>
        </div>
        <div id="calendar">
            <div v-for="week in weeks" class="calendar-week" :key="week">
                <calendar-day v-for="day in week" :day="day" :key="day">{{ day }}</calendar-day>
            </div>
        </div>
    </div>
</template>
<script>
    import CalendarDay from './CalendarDay.vue';
    export default {
       data(){
           return {
              month: 1,
              year: 2019,
           }
       },
       computed:{
            days(){
                let days = [];
                let currentDay = this.$moment(`${this.year}-${this.month}-1`, 'YYYY-M-D');
                do {
                    days.push(currentDay);
                    currentDay = this.$moment(currentDay).add(1, 'days');
                }while((currentDay.month() + 1) === this.month);
                // Add previous days to start
                currentDay = this.$moment(days[0]);

                while(currentDay.day() !== 1){
                    currentDay = this.$moment(currentDay).subtract(1, 'days');
                    days.unshift(currentDay);
                }

                // Add following days to end
                currentDay = this.$moment(days[days.length - 1]);

                while(currentDay.day() !== 0){
                    currentDay = this.$moment(currentDay).add(1, 'days');
                    days.push(currentDay);
                }

                return days;
            },
            weeks(){
                let weeks = [];
                let week = [];

                for(let day of this.days){
                    week.push(day);
                    if(week.length === 7){
                        weeks.push(week);
                        week = [];
                    }
                }
                return weeks;
            }
        },
        components:{
            CalendarDay
        }
    } 
</script>
