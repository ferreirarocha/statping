<template>
    <div class="row">
        <div v-for="(incident, i) in incidents" class="col-12 mt-4">
            <h5>Incident: {{incident.title}}<span class="font-2 float-right">{{niceDate(incident.created_at)}}</span></h5>
            {{incident.description}}
            <div class="row">
                <div v-for="(update, i) in incident.updates.reverse()" v-bind:key="update.id" class="col-12 mt-3">
                    <span class="col-2 badge text-uppercase" :class="badgeClass(update.type)">{{update.type}}</span>
                    <span class="col-10">{{update.message}}</span>
                    <span class="col-12 font-1 float-right text-black-50">{{ago(update.created_at)}} ago</span>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import Api from '../../API';

export default {
  name: 'IncidentsBlock',
    props: {
        service: {
            type: Object
        }
    },
    data() {
        return {
            incidents: null,
        }
    },
    mounted () {
        this.getIncidents()
    },
    methods: {
        badgeClass(val) {
          switch (val.toLowerCase()) {
            case "resolved":
              return "badge-success"
            case "update":
              return "badge-info"
            case "investigating":
              return "badge-danger"
          }
        },
        async getIncidents() {
            this.incidents = await Api.incidents_service(this.service)
        }
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
