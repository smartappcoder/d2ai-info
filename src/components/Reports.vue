<script setup lang="ts">
  import { ref, onMounted, onBeforeMount } from 'vue'
  import VueMarkdown from 'vue-markdown-render'
  import { downloadData, list } from 'aws-amplify/storage';
  import FileSaver from 'file-saver'
  // Define the type of the objects in the array
  interface Report {
    name: string;
    title: string;
    abstract: string;
    pdf: string;
  }
  let reports = ref<Report[]>([])
  onMounted(async () => {
    try {
        const listResult = await list({
	        path: 'reports/',
          options:{
            subpathStrategy: { strategy:'exclude' }
          }
        });
        // console.log(listResult)
        if (listResult.items.length > 1) { // means there are some report files
          for (let item of listResult.items) {
            // console.log(item)
            let filePath = item.path
            let filePathParts = filePath.split('.')
            if (filePathParts.length >= 2) {
              let reportFileFullNameWithPath = filePathParts[0]
              let reportFileType = filePathParts[1]
              if (reportFileType === 'json') { // The metafile for a report is stored as a JSON
                let reportFileName = reportFileFullNameWithPath.split('reports/')[1]
                let report: Report = {
                  name: '',
                  abstract: '',
                  pdf: '',
                  title: ''
                } 
                report.name = reportFileName
                const result = await downloadData({
                  path: `reports/${reportFileName}.json`,
                  options: {
                    // Specify a target bucket using name assigned in Amplify Backend
                    bucket: "d2aiInfoStorage"
                  }
                }).result;
                
                let reportMetaJsonStr = await result.body.text()
                // console.log(reportMetaJsonStr)
                let reportMetaJson = JSON.parse(reportMetaJsonStr)
                const reportAbstractResult = await downloadData({
                  path: `reports/${reportFileName}/${reportMetaJson.abstract}`,
                  options: {
                    // Specify a target bucket using name assigned in Amplify Backend
                    bucket: "d2aiInfoStorage"
                  }
                }).result;
                report.abstract = await reportAbstractResult.body.text()
                report.pdf = reportMetaJson.pdf
                report.title = reportMetaJson.title
                reports.value.push(report) 
              }
            }
          }
          console.log(reports)
        }
      } catch (error) {
        console.log(`Error: ${error}`)
      }
  })
  const downloadClick = async (report:Report) => {
      let downloadPath = `reports/${report.name}/${report.pdf}`
      console.log('downloadClick: ', downloadPath)
      try {
        const result = await downloadData({
          path: downloadPath,
          
          options: {
            // Specify a target bucket using name assigned in Amplify Backend
            bucket: "d2aiInfoStorage"
          }
        }).result;
        let blob = await result.body.blob()
        FileSaver.saveAs(blob, report.pdf);
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
          <v-col v-if="reports.length > 0">
            <v-card v-for="(report) in reports" :key="report.name" elevation="2" class="pa-10" outlined>
              <v-row align="center" justify="center">
                <h1 class="display-1">{{ report.title }}</h1>
              </v-row>
              <v-row style="border: 1px solid #000; height: 200px; overflow: auto;">
                <v-col class="ml-4 mr-4">
                  <vue-markdown class="mt-4" :source="report.abstract">
                  </vue-markdown>
                </v-col>
              </v-row>
              <v-row class="mt-6" align="center" justify="center">
                <v-btn color="primary" large @click="downloadClick(report)">Download</v-btn>
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