<template>
  <div class="card-deck">
    <div class="card text-left">
        <div class="card-body">
            <h5 class="card-title">{{item.title}}</h5>
            <p class="card-text"><small class="text-muted">{{item.resolution}}</small></p>
            <p class="card-text">Source Incident ID :
                <a :href="'https://ing.service-now.com/incident.do?sysparm_query=number=' + item.source_incident_id" target="_blank">
                    {{item.source_incident_id}}
                </a>
            </p>
            <p class="card-text">Description : <small class="text-muted">{{description}}</small></p>
            <p class="card-text">Score : <small class="text-muted">{{item.score}}</small></p>
        </div>
        <div class="card-footer">
            <p class="card-text">Date : <small class="text-muted">{{item.started_at}}</small></p>
        </div>
    </div>
  </div>
</template>

<script>

export default {
    name: 'item',
    props: ['item'],
    data() {
        return {
            description: ''
        }
    },
    filters: {
        shortDescription(value) {
        if (value && value.length > 100) {
            return value.substring(0, 100) + '...';
        } else {
            return value;
        }
        }
    },
    created() {
        this.getDescription(this.item.source_incident_id);
    },
    computed: {
        /* description() {
            console.log("Here it is : ", this.$store.state.description);
            return this.$store.state.description;
        } */
    },
    methods: {
        async getDescription(payload) {
        const GET_url = this.$store.state.endpoints.descriptionURL + payload;
        const GETrequestOptions = {
        method: "GET",
        headers: { "Target-URL": this.$store.state.endpoints.description_targetURL + payload, "Authorization": "Bearer " + this.$store.state.user.accessToken, "accept": "application/json", "Content-Type": "application/json" },
        // headers: { "Target-URL": this.state.endpoints.targetURL, "Authorization": "Bearer " + this.state.user.accessToken, "accept": "application/json", "Content-Type": "application/json" },
        };      
        const description = await fetch(GET_url, GETrequestOptions);
        const desc = await description.json();
        //console.log(desc.hits.hits['0']._source.description);
        this.description = desc.hits.hits['0']._source.description;
     }
    }
}
</script>

<style scoped>

body {
    background-color: #E7E9F5;
    font-family: 'Heebo', sans-serif
}

.card {
    width: 600px;
    border: 1.5px solid #E3E6ED;
    border-radius: 10px;
    margin: 5px 5px 5px 5px
}

.form-control {
    border-radius: 7px;
    border: 1.5px solid #E3E6ED
}

input.form-control:focus {
    box-shadow: none;
    border: 1.5px solid #E3E6ED;
    background-color: #F7F8FD;
    letter-spacing: 1px
}

.btn-primary {
    background-color: #5878FF !important;
    border-radius: 7px
}

.btn-primary:focus {
    box-shadow: none
}

.text {
    font-size: 13px;
    color: #9CA1A4
    
}

.card-text {
    margin: 0 0 0 0;
}

.flex-row {
    border: 1px solid #F2F2F4;
    border-radius: 10px;
    margin: 0 1px 0
}

.flex-column p {
    font-size: px
}

span.mb-2 {
    font-size: 12px;
    text-align: left;
    color: #8896BD
}

h5 span {
    color: #869099
}

@media screen and (max-width: 1024px) {
    .card {
        display: flex;
        justify-content: center;
        text-align: center
    }
    
    .price {
        border: none;
        margin: 0 auto
    }
}
</style>