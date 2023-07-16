<template>
    <div>
        <h2 class="font-bold">Pont Chaban</h2>
        <div>
            <div v-for="r in futureRecords" class="p-1">
                <div class="card flex justify-between">
                    <div>
                    <i class="material-icons mr-2">calendar_today</i>{{ formatDate(r.fields.date_passage) }}
                    <p><i class="material-icons mr-2">schedule</i>De {{ r.fields.fermeture_a_la_circulation }} à {{ r.fields.re_ouverture_a_la_circulation }}</p>
                    <p><i class="material-icons mr-2">directions_boat</i>Bateau : {{ r.fields.bateau }}</p>
                </div>
 <!-- TODO : Handle the case when endTime is not same day as startTime -->
                    <add-to-calendar-button class=""
                        name="Fermeture pont Chaban"
                        options="'Apple','Google','Microsoft365','ical','Outlook.com'"
                        location="Pont Chaban delmas, Bordeaux, France"
                        :startDate="r.fields.date_passage"
                        :startTime="r.fields.fermeture_a_la_circulation"
                        :endTime="r.fields.re_ouverture_a_la_circulation"
                        :description="'Passage du bateau ' + r.fields.bateau"
                        timeZone="Europe/Paris"
                        label="Ajouter à mon calendrier"
                        >
                    </add-to-calendar-button>
                </div>
            </div>
      </div>
    </div>
</template>

<script setup>
    import { ref } from 'vue'
    import 'add-to-calendar-button';

    //  fetch the records
    const { data: records } = await useFetch('https://opendata.bordeaux-metropole.fr/api/records/1.0/search/?dataset=previsions_pont_chaban&q=&rows=50')
    
    // Remove the events from the past
    const today = new Date();
    today.setHours(0,0,0,0); // Set time to 00:00:00

    const futureRecords = computed(() => {
        return records.value.records.filter((r) => {
            const recordDate = new Date(r.fields.date_passage);
            recordDate.setHours(0,0,0,0);
            return recordDate >= today;
        });
    });


    // method to format date
    const formatDate = (dateString) => {
        const options = { weekday: 'long', month: 'long', day: 'numeric' } // Required format
        const date = new Date(dateString)
        return date.toLocaleDateString(undefined, options)
    }
    // Format date for calendar
    const formatDateForCalendar = (dateString) => {
        const options = { year: 'numeric', month: 'long', day: 'numeric' } // Required format
        const date = new Date(dateString)
        return date.toLocaleDateString(undefined, options)
}
</script>

<style scoped>

</style>