<template>
  <base-card>
    <base-button @click="setSelectedTab('stored-resources')" :class="storedResButtonMode">Stored Resources</base-button>
    <base-button @click="setSelectedTab('add-resource')" :class="addResButtonMode">Add Resource</base-button>
  </base-card>
  <keep-alive>
    <component :is="selectedTab"></component>
  </keep-alive>
</template>

<script>
import AddResource from './AddResource.vue';
import StoredResources from './StoredResouces.vue';

  export default {
    components: {
      AddResource,
      StoredResources
    },
    data() {
      return {
        selectedTab: 'stored-resources',
        storedResources: [
          {
            id: 'official-guide',
            title: 'Official Guide',
            description: 'The description 1',
            link: 'https://vuejs.org'
          },
          {
            id: 'google',
            title: 'Google',
            description: 'The description 2',
            link: 'https://google.com'
          },
        ],
      };
    },
    provide() {
      return {
        resources: this.storedResources,
        addResource: this.addResource,
        deleteResource: this.removeResource
      }
    },
    computed: {
      storedResButtonMode() {
        return this.selectedTab === 'stored-resources' ? null : 'flat';
      },
      addResButtonMode() {
        return this.selectedTab == 'add-resource' ? null : 'flat';
      }
    },
    methods: {
      setSelectedTab(tab) {
        this.selectedTab = tab;
      },
      addResource(title, description, url) {
        const newResource = {
          title: title,
          description: description,
          link: url,
        };

        this.storedResources.unshift(newResource);
        this.selectedTab = 'stored-resources';
      },
      removeResource(resId) {
        const resIndex = this.storedResources.findIndex(res => res.id == resId);
        this.storedResources.splice(resIndex, 1);
      }
    }
  }
</script>
