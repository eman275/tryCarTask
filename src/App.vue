<template>
  <div id="app">
    <main>
      <PaginationData
        v-if="tableData"
        :totalRecords="tableData.length"
        :perPageOptions="perPageOptions"
        v-model="pagination"
      />
      <PostsData
        v-if="tableData"
        :theData="computedTableData"
        :config="config"
        :style="{height: '600px'}"
      />
    </main>
  </div>
</template>

<script>
import PostsData from './components/PostsData'
import PaginationData from './components/PaginationData'
import { defineComponent } from 'vue'
import axios from 'axios'
const perPageOptions = [20, 50, 100]

  export const fetchDataSuccess = (data ) => {
  return {
    type: "FETCH_DATA_SUCCESS",
    payload: data,
  };
};
export default defineComponent( {
  components: {
    PostsData,
    PaginationData
  },
  data () {
    return {
      perPageOptions,
      tableData: undefined,
      pagination: { page: 1, perPage: perPageOptions[0] },
      config: [
         {
          key: 'image',
          title: 'image',
          type: 'image'
        },
        {
          key: 'title',
          title: 'title',
          type: 'text'
        },
        {
          key: 'userName',
          title: 'user Name',
          type: 'text'
        },
        {
          key: 'phoneNumber',
          title: 'phone Number',
          type: 'text'
        },
       
       
      ]
    }
  },

  mounted () {
   axios
     .get('https://63bb2aff32d17a509089126c.mockapi.io/userposts/posts')
     .then(response => {
       this.tableData = response.data
       console.log('tableData', this.tableData)
     })
    },
  computed: {
    computedTableData () {
      if (!this.tableData) return []
      else {
        const firstIndex = (this.pagination.page - 1) * this.pagination.perPage
        const lastIndex = this.pagination.page * this.pagination.perPage

        return this.tableData.slice(firstIndex, lastIndex)
      }
    }
  },
  
})
</script>

<style>

body {
  font-family: Helvetica, sans-serif;
  font-weight: 400;
  margin: 0;
}

main {
  margin: 30px;
  height: 85vh;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}

nav {
  height: 60px;
  background: #222;
  font-size: 32px;
  color: white;
  display: flex;
  align-items: center;
  padding-left: 20px;
}
</style>
