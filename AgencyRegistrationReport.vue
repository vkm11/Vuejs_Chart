<template>
  <vx-card>
      <h4 class="mb-4">Agency Registration Report</h4>

      <div class="vx-row">
        <div class="vx-col sm:w-1/3 w-full mb-4" >
          <label class="vs-input--label">Start Date</label>
          <datepicker format="yyyy-MM-dd" placeholder="YYYY-MM-DD" v-model="reportForm.startdate"></datepicker>
        </div>
        <div class="vx-col sm:w-1/3 w-full mb-4" >
          <label class="vs-input--label">End Date</label>
          <datepicker format="yyyy-MM-dd" placeholder="YYYY-MM-DD" v-model="reportForm.enddate"></datepicker>
        </div>


        <div class="flex flex-wrap items-center justify-start">
          <vs-button class="mr-2" v-bind:disabled="$v.reportForm.$invalid" @click="fetchreport">Get Report</vs-button>
          <!-- <vs-button class="mr-2"  @click="fetchreport">Get Report</vs-button> -->
        </div>
      </div>
      
      <div class="mb-8"> 
        <vs-table :data="reports">
          <template slot="thead"> 
            <vs-th>Date</vs-th> 
            <vs-th>Total Registration</vs-th> 
          </template> 
          <template slot-scope="{data}"> 
            <vs-tr :key="index" v-for="(tr, index) in data">
              <vs-td >{{ tr.date}}</vs-td>
              <vs-td>{{ tr.total_agencies }}</vs-td>
            </vs-tr> 
          </template> 
        </vs-table> 
      </div>
      
      <!-- Chart Start -->
      <div class="mb-8">
        <line-chart-agency :chart="chartData" :key="chartData.length" />
      </div>
      <!-- Chart End -->


  </vx-card>
</template>

<script>
import { required } from 'vuelidate/lib/validators';
import Datepicker from 'vuejs-datepicker'
import axios from 'axios'; 
import LineChartAgency from './LineChartAgency.vue'

export default {
  name: 'Agency-Report',
   components: { 
    Datepicker,
    LineChartAgency
  },
  
  data() {
    return {
      reportForm: {
        startdate: '',
        enddate: '',
      },
      reports:[],
      
      // Chart start
      chartData:[],
      chartnewdata: {
        date: [],
        agencies: [],
      },
    // chart end

    }
  },
  mounted() {
    this.fillData();
  },
  validations: {
    reportForm : {
      startdate: { required },
      enddate: { required },
    }
  },
  methods: {
    // fetchreport(){
    //   axios.post('/agency/registrations', this.reportForm)
    //   .then((response) => {
    //     this.reports = response.data.result.data;
    //   });
    // },
    fetchreport(){
      axios.post('/agency/registrations', this.reportForm)
      .then((response) => {
        this.reports = JSON.parse(JSON.stringify(response.data.result.data));
        for(let i=0; i<this.reports.length; i++){
          this.chartnewdata.date.push(this.reports[i].date);
          this.chartnewdata.agencies.push(this.reports[i].total_agencies);
        }
        this.chartData=this.chartnewdata;
      });
    },
  },
}  
</script>
