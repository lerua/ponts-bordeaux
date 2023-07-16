<template>
    <div>
        <h2 class="font-bold">Pont Chaban</h2>
        <div>
            <div v-for="r in records.records" class="p-1">
                <div class="card">
                    {{ formatDate(r.fields.date_passage) }}
                    <p>De {{ r.fields.fermeture_a_la_circulation }} à {{ r.fields.re_ouverture_a_la_circulation }}</p>
                    <p>Bateau : {{ r.fields.bateau }}</p>
                    {{r.fields.date_passage}}
                    <add-to-calendar-button
                        name="Fermeture pont Chaban"
                        options="'Apple','Google','Microsoft365','ical'"
                        location="Pont Chaban delmas, Bordeaux, France"
                        :startDate="r.fields.date_passage"
                        :startTime="r.fields.fermeture_a_la_circulation"
                        :endTime="r.fields.re_ouverture_a_la_circulation"
                        :description="'Passage du bateau ' + r.fields.bateau"
                        timeZone="Europe/Paris"
                        ></add-to-calendar-button>
                </div>
            </div>
      </div>
    </div>
</template>

<script setup>
    import { ref } from 'vue'
    import 'add-to-calendar-button';

    //  fetch the records
    const { data: records } = await useFetch('https://opendata.bordeaux-metropole.fr/api/records/1.0/search/?dataset=previsions_pont_chaban&q=&rows=20')
    
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