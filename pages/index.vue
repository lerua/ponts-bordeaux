<template>
    <div>
        <h2>Pont Chaban</h2>

        <div class="grid grid-cols-4 gap-5">
            <div v-for="r in records.records">
                {{ r.fields.date_passage }}
                <p>De {{ r.fields.fermeture_a_la_circulation }} à {{ r.fields.reouverture_a_la_circulation }}</p>
                <p>Bateau : {{ r.fields.bateau }}</p>
            </div>
      </div>
    </div>
</template>

<script setup>
       //  fetch the records
       const { data: records } = await useFetch('https://opendata.bordeaux-metropole.fr/api/records/1.0/search/?dataset=previsions_pont_chaban&q=&rows=20')
</script>

<style scoped>

</style>