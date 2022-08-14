<template>
  <base-card>
    <base-button @click="setSelectedTab('stored-resource')"
    :mode="storedResButtonMode"
      >Stored Resources</base-button
    >
    <base-button @click="setSelectedTab('add-resource')"
    :mode="addResButtonMode"
      >Add Resources</base-button
    >
  </base-card>
  <keep-alive>
    <component :is="selectedTab"></component>
  </keep-alive>
</template>

<script>
import StoredResource from './StoredResource.vue';
import AddResource from './AddResource.vue';

export default {
  components: {
    StoredResource,
    AddResource,
  },
  data() {
    return {
      selectedTab: 'stored-resource',
      storedResources: [
        {
          id: 'official-document',
          title: 'Official Document',
          description: 'Official Document Created By Evan You',
          link: 'www.vue.com/docs',
        },
        {
          id: 'official-document',
          title: 'Official Document',
          description: 'Official Document Created By Evan You',
          link: 'www.vue.com/docs',
        },
      ],
    };
  },
  computed: {
    storedResButtonMode(){
        return this.selectedTab == 'stored-resource' ? null : 'flat';
    },
    addResButtonMode(){
        return this.selectedTab == 'add-resource' ? null : 'flat';
    }
  },
  provide() {
    return {
        resources: this.storedResources,
        addResource: this.addResource,
        removeResource: this.removeResource,
    }
  },
  methods: {
    setSelectedTab(tab) {
      this.selectedTab = tab;
    },
    addResource(title, description, link){
        const newRes = {
            id: new Date().toISOString(),
            title: title,
            description: description,
            link: link
        };

        this.storedResources.unshift(newRes);
        this.selectedTab = 'stored-resource';
    },
    removeResource(id){
        const removeIndex = this.storedResources.findIndex(res => res.id === id);
        this.storedResources.splice(removeIndex,1);
    }
  },
};
</script>