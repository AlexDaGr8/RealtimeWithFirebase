<template>
  <div id="app">
    <Notebook @change-page="changePage" @new-page="newPage" :pages="pages" :activePage="index" />
    <Page @save-page="savePage" @delete-page="deletePage" @update-page="updatePage" :page="pages[index]" />
  </div>
</template>

<script>
import Notebook from './components/Notebook'
import Page from './components/Page'
import database from './components/firebase'

export default {
  name: 'app',
  components: {
    Notebook,
    Page
  },
  data: () => ({
    //pages: [],
    index: 0
  }),
  firebase () {
    return {
      pages: database
    }
  },
  methods: {
    newPage () {
      this.pages.push({
        title: '',
        content: ''
      })
      this.index = this.pages.length - 1
    },
    changePage (index) {
      this.index = index
    },
    savePage (page) {
      if (page['.key']) {
        this.updateExistingPage(page)
      } else {
        this.insertNewPage(page)
      }
    },
    deletePage (page) {
      if (page['.key']) {
        database.child(page['.key']).remove()
      } else {
        this.pages.splice(this.index, 1);
        this.index = -1;
      }
    },
    updatePage (page) {
      if (page['.key']) {
        this.updateExistingPage(page)
      }
    },
    updateExistingPage (page) {
      const copy = {...page}
      delete copy['.key']
      database.child(page['.key']).set(copy)
    },
    insertNewPage (page) {
      var tempPage = this.pages.filter(d => !d['.key'])[0]
      var tmpInd = this.pages.indexOf(tempPage)
      this.pages.splice(tmpInd, 1);
      database.push(page)
    }
  }
}
</script>
<style>
    html, body, #app {
        height: 100%;
    }

    body {
        margin: 0;
    }

    #app {
        font-family: 'Avenir', Helvetica, Arial, sans-serif;
        display: flex;
        flex-direction: row;
    }
</style>