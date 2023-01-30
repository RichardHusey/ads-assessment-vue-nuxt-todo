<template>
  <v-container class="pa-4" fluid>
    <v-card>
      <v-card-title>
        Domains
        <div class="flex-grow-1" />
        <v-text-field v-debounce:700ms="handleInputDebounce" append-icon="mdi-magnify" label="Search" single-line
          hide-details />
        <!-- <v-text-field v-model="search" append-icon="mdi-magnify" label="Search" single-line hide-details /> -->

      </v-card-title>
      <v-data-table :headers="headers" :items.sync="domains" :loading="loading" disable-sort>
        <template v-slot:body="{ items }">
          <tbody>
            <tr v-for="(item, i)  in items" :key="item.id">
              <td>
                {{ item.domain_name }}
              </td>
              <td>{{ new Date(item.time_added_to_user).toLocaleString() }}</td>
              <td>{{ item.is_idn }}</td>
              <td>{{ item.imprint }}</td>
              <td>
                <v-btn icon @click="handleImprint(item, i)">
                  <v-icon :style="{ color: item.imprint ? 'blue' : 'red' }">mdi-fingerprint</v-icon>
                </v-btn>
                <v-btn icon @click="handleDelete(item, i)">
                  <v-icon>mdi-delete</v-icon>
                </v-btn>
              </td>
            </tr>
          </tbody>
        </template>
      </v-data-table>
    </v-card>
    <v-btn fab color="primary" bottom right absolute @click="showDialog = !showDialog">
      <v-icon>mdi-plus</v-icon>
    </v-btn>
    <v-dialog v-model="showDialog" max-width="600px">
      <AddDomain @refresh="fetchDomains()" />
      <!-- TODO: Load the <strong>Create Domain</strong> component into the dialog in domains page. -->
      <div v-if="loading">
        ...
      </div>
    </v-dialog>
  </v-container>
</template>

<script>
import AddDomain from '../components/AddDomain.vue';

export default {
  components: {
    AddDomain,
  },
  data() {
    return {
      loading: false,
      search: '',
      showDialog: false,
      headers: [
        { text: 'Domain Name', value: 'domain_name', },
        { text: 'Created At', value: 'time_added_to_user' },
        { text: 'Internationalized', value: 'is_idn' },
        { text: 'Imprinted', value: 'imprint' },
      ],
      domains: [],
    }
  },

  mounted() {
    this.fetchDomains();
  },

  methods: {
    /**
     * @todo: Set loading state while call is being process.
     */
    async fetchDomains() {
      try {
        this.isLoading = true;
        this.domains = (await this.$axios.get(`/domains`)).data;
        this.showDialog = false;
      } catch (e) {
        console.log(e);
      }
      finally {
        this.isLoading = false;
      }
    },

    // @todo: Manages the input debounce handler

    async handleInputDebounce(value) {
      if (value) {
        await this.handleSearch(value);
      } else {
        await this.fetchDomains();
      }
    },

    /**
     * @todo: Make the search input field filter the domains displayed by asynchronously querying api endpoint GET | /domains?domain_name={domain}.
     */
    async handleSearch(value) {
      try {
        this.domains = (await this.$axios.get(`/domains?domain_name=${value}`)).data;
      } catch (error) {
        console.log(error);
      }
    },

    /**
     * @todo: Make the imprint icon toggle the imprint flag for a domain by changing icon color when true/false. PUT | /domains/{id}
     */
    async handleImprint(item, index) {
      try {
        await this.$axios.put(`/domains/${item.id}`, {
          ...item,
          imprint: !item.imprint,
        });
        this.fetchDomains();
      } catch (error) {
        console.log(error);
      }
    },

    /**
     * @todo: Make the delete icon remove domain using api endpoint DELETE | /domains/{id}
     */
    async handleDelete(item, index) {
      try {
        await this.$axios.delete(`/domains/${item.id}`);
        this.fetchDomains();
      } catch (error) {
        console.log(error);
      }

    },
  }
}
</script>
