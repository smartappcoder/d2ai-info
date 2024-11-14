<script setup lang="ts">
  import VueMarkdown from 'vue-markdown-render'
  import { downloadData } from 'aws-amplify/storage';
  const downloadClick = () => {
      console.log('downloadClick')
      try {
        const result = downloadData({
          path: "reports/test.pdf",
          
          options: {
            // Specify a target bucket using name assigned in Amplify Backend
            bucket: "d2aiInfoStorage"
          }
          
        }).result;
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
                <h1 class="display-1">Medical IoT for diabetes</h1>
              </v-row>
              <v-row style="border: 1px solid #000; height: 200px; overflow: auto;">
                <v-col class="ml-4 mr-4">
                <vue-markdown class="mt-4" :source="`## Executive Summary

The integration of the Internet of Things (IoT) into healthcare has revolutionized the management of chronic diseases, particularly diabetes. This executive summary provides a concise overview of the report, highlighting key findings, market trends, and strategic recommendations in the realm of medical IoT for diabetes management.

### Key Findings

- **Prevalence of Diabetes**: Diabetes remains a significant global health challenge, with an increasing prevalence due to factors such as aging populations, sedentary lifestyles, and rising obesity rates. The World Health Organization (WHO) estimates that the number of people with diabetes has risen from 108 million in 1980 to 422 million in 2014, and this number is expected to continue growing.

- **Technological Advancements**: The advent of IoT technology has opened new avenues for diabetes management. IoT-enabled devices, such as continuous glucose monitors (CGMs), insulin pumps, and smart glucometers, have enhanced the ability to monitor and manage blood glucose levels in real-time. These devices facilitate better glycemic control, reduce the risk of complications, and improve the quality of life for patients.

- **Data-Driven Insights**: IoT devices generate vast amounts of data that can be analyzed to provide actionable insights for both patients and healthcare providers. Big data analytics and artificial intelligence (AI) are being leveraged to predict trends, personalize treatment plans, and optimize diabetes management strategies.

- **Patient-Centric Care**: IoT technology empowers patients to take a more active role in their diabetes management. Mobile applications and telecare services enable remote monitoring, virtual consultations, and personalized feedback, fostering a patient-centric approach to care.

- **Challenges and Concerns**: Despite the benefits, the implementation of IoT in diabetes care faces challenges such as data security and privacy concerns, interoperability issues, and the need for robust regulatory frameworks. Addressing these challenges is crucial for the widespread adoption of IoT solutions in healthcare.

### Market Trends

- **Growth of the Digital Diabetes Management Market**: The global digital diabetes management market was valued at USD 11.8 billion in 2022 and is projected to expand at a compound annual growth rate (CAGR) of 8.3% from 2023 to 2030. This growth is driven by the increasing prevalence of diabetes, technological advancements, and the rising demand for medical devices and management apps.

- **Emergence of 5G and AI**: The development of 5G networks and AI technologies is set to further enhance IoT applications in diabetes care. These technologies enable faster data transmission, improved connectivity, and more sophisticated data analysis, leading to more efficient and effective diabetes management solutions.

- **Personalized Healthcare Systems**: There is a growing trend towards personalized healthcare systems that leverage IoT and AI to tailor treatment plans to individual patient needs. This approach improves patient outcomes and satisfaction by providing more targeted and effective interventions.

### Strategic Recommendations

1. **Enhance Data Security and Privacy**: To build trust and ensure the safe use of IoT devices, it is essential to implement robust data security measures and comply with privacy regulations. This includes encryption, secure data storage, and transparent data-sharing practices.

2. **Promote Interoperability**: Developing standardized protocols and frameworks for interoperability will facilitate seamless integration of IoT devices with existing healthcare systems. This will enable more comprehensive data sharing and collaboration among healthcare providers.

3. **Invest in Research and Development**: Continued investment in R&D is crucial to drive innovation and improve the functionality and accuracy of IoT devices for diabetes management. This includes exploring new sensor technologies, energy harvesting methods, and AI algorithms.

4. **Foster Patient Engagement**: Encourage patient engagement by providing user-friendly interfaces, personalized feedback, and educational resources. Empowering patients to actively participate in their care can lead to better adherence to treatment plans and improved health outcomes.

5. **Collaborate with Key Stakeholders**: Collaboration among healthcare providers, technology companies, regulatory bodies, and patients is essential to address challenges and drive the successful implementation of IoT solutions in diabetes care.

In conclusion, the integration of IoT technology into diabetes management presents significant opportunities to improve patient outcomes and streamline healthcare delivery. By addressing challenges and leveraging technological advancements, stakeholders can harness the full potential of IoT to transform diabetes care.`"
                  >
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