<template>
  <div>
    <h1>Garage Management</h1>
    <button @click="openForm(null)">Create Garage</button>
    <table>
      <thead>
        <tr>
          <th>Title</th>
          <th>Location</th>
          <th>Actions</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="garage in garages" :key="garage.id">
          <td>{{ garage.title }}</td>
          <td>{{ garage.location }}</td>
          <td>
            <button @click="openForm(garage)">Edit</button>
          </td>
        </tr>
      </tbody>
    </table>
    <GarageForm
      v-if="showForm"
      :garage="currentGarage"
      @submit="handleSubmit"
      @cancel="closeForm"
    />
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import GarageForm from '@/components/GarageForm.vue';
import ProjectService, { type GarageItem } from '@/services/ProjectService';

export default defineComponent({
  components: {
    GarageForm,
  },
  data() {
    return {
      garages: [] as GarageItem[],
      showForm: false,
      currentGarage: null as GarageItem | null,
      projectId: 'project123', // Replace with actual project ID
      propertyId: 'property456', // Replace with actual property ID
      buildingId: 'building789', // Replace with actual building ID
    };
  },
  mounted() {
    this.fetchGarages();
  },
  methods: {
    fetchGarages() {
      ProjectService.getGarages(this.projectId, this.propertyId, this.buildingId)
        .then((garages: GarageItem[]) => {
          this.garages = garages;
        })
        .catch((error) => {
          console.error('Error fetching garages:', error);
        });
    },
    openForm(garage: GarageItem) {
      this.currentGarage = garage || null;
      this.showForm = true;
    },
    closeForm() {
      this.showForm = false;
      this.currentGarage = null;
    },
    handleSubmit(data: any) {
      if (this.currentGarage && this.currentGarage.id) {
        // Update existing garage
        ProjectService.updateGarage(
          this.projectId,
          this.propertyId,
          this.buildingId,
          this.currentGarage.id,
          data
        )
          .then(() => {
            this.fetchGarages();
            this.closeForm();
          })
          .catch((error) => {
            console.error('Error updating garage:', error);
          });
      } else {
        // Create new garage
        ProjectService.createGarage(
          data.title,
          this.projectId,
          this.propertyId,
          this.buildingId
        )
          .then(() => {
            this.fetchGarages();
            this.closeForm();
          })
          .catch((error) => {
            console.error('Error creating garage:', error);
          });
      }
    },
  },
});
</script>
  