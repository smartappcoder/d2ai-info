<script setup lang="ts">
  import { ref, onMounted, onBeforeMount } from 'vue'
  import VueMarkdown from 'vue-markdown-render'
  import { downloadData } from 'aws-amplify/storage';
  import FileSaver from 'file-saver'
  let reportMetaJson = ref({
    title: '',
    abstract: '',
    pdf: ''
  })
  let reportAbstractMarkdown = ref('')
  let reportName = 'RP-medical IoT for diabetes-th13-rev2-962'

  onMounted(async () => {
    try {
        const result = await downloadData({
          path: `reports/${reportName}.json`,
          options: {
            // Specify a target bucket using name assigned in Amplify Backend
            bucket: "d2aiInfoStorage"
          }
        }).result;
        let reportMetaJsonStr = await result.body.text()
        // console.log(reportMetaJsonStr)
        reportMetaJson.value = JSON.parse(reportMetaJsonStr)

        const reportAbstractResult = await downloadData({
          path: `reports/${reportMetaJson.value.abstract}`,
          options: {
            // Specify a target bucket using name assigned in Amplify Backend
            bucket: "d2aiInfoStorage"
          }
        }).result;
        reportAbstractMarkdown.value = await reportAbstractResult.body.text()
      } catch (error) {
        console.log(`Error: ${error}`)
      }
  })
  const downloadClick = async () => {
      console.log('downloadClick')
      try {
        const result = await downloadData({
          path: `reports/${reportMetaJson.value.pdf}`,
          
          options: {
            // Specify a target bucket using name assigned in Amplify Backend
            bucket: "d2aiInfoStorage"
          }
        }).result;
        let blob = await result.body.blob()
        let fileName = 'test.pdf'
        FileSaver.saveAs(blob, reportMetaJson.value.pdf);
      } catch (error) {
        console.log(`Error: ${error}`)
      }
  };
</script>
<template>
  <v-app>
    <!-- Navigation bar -->
    <v-app-bar app color="primary" dark>
      <v-toolbar-title>AI Market Research</v-toolbar-title>
      <v-spacer></v-spacer>
      <v-btn>Home</v-btn>
      <v-btn>About</v-btn>
    </v-app-bar>

    <!-- Main content with a hero section -->
    <v-main>
      <v-container>
        <v-row>
          <v-col>
            <v-card elevation="2" class="pa-10" outlined>
              <v-row align="center" justify="center">
                <h1 class="display-1">{{ reportMetaJson.title }}</h1>
              </v-row>
              <v-row style="border: 1px solid #000; height: 200px; overflow: auto;">
                <v-col class="ml-4 mr-4">
                  <vue-markdown class="mt-4" :source="reportAbstractMarkdown">
                  </vue-markdown>
                </v-col>
              </v-row>
              <v-row class="mt-6" align="center" justify="center">
                <v-btn color="primary" large @click="downloadClick">Download</v-btn>
              </v-row>
            </v-card>
          </v-col>
        </v-row>
      </v-container>
    </v-main>

    <!-- Footer -->
    <v-footer app color="primary" dark>
      <v-col class="text-center white--text">© 2024 D2AI</v-col>
    </v-footer>
  </v-app>
</template>
<script setup lang="ts">
</script>
<script lang="ts">
  export default {
    name: 'Reports'
  };
</script>

<style>
/* Optional custom styling */
.v-application {
  font-family: 'Roboto', sans-serif;
}
</style>