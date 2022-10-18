<template>
  <div>
      <table>
       <thead >
        <tr>
          <th>Data</th>
          <th @click="sortByName">Name</th>
          <th @click="sortByCount">Count</th>
          <th @click="sortByDictans">Dictans</th>
        </tr>
      </thead>
      <tbody>
       <tr v-for = "(item, index) of pagesItems" v-bind:key = "index">
        <td>{{new Date(item.data ).toLocaleDateString()}}</td>
        <td>{{ item.title }}</td>
        <td>{{ item.count }}</td>
        <td>{{ item.distans }}</td>
      </tr>
      </tbody>
    </table>
    <div class ="navigator">
        <div class = "page"
          v-for = "page in pages"
          :key="page"
          :class="{'page_activ': page === pageNumber}"
          @click="pageClick(page)"> {{page}}
        </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: 'HelloWorld',
  data() {
    return {
      items: [],
      itemsPage: 10,
      pageNumber: 1,
      isElVisible: false,
    };
  },
  async created() {
    try {
      const res = await axios.get(`http://localhost:3000/posts`);
      this.items = res.data;
    } catch (error) {
      console.log(error);
    }
  },
    computed:{
      pages(){
        return Math.ceil(this.items.length / 10);
      },
      pagesItems(){
        let from =  (this.pageNumber - 1) * this.itemsPage;
        let to = from + this.itemsPage
        return this.items.slice(from, to)
      },
      
    },
    methods: {
      pageClick(page){
        this.pageNumber = page
      },
      buttonClick(){
        this.isElVisible = !this.isElVisible
      },
      sortByCount(){
        this.items.sort((a, b) => a.count - b.count)
      },
      sortByName(){
        this.items.sort((a, b) => a.title.localeCompare(b.title))
      },
      sortByDictans(){
        this.items.sort((a, b) => a.distans - b.distans)
      },
      }
    }
</script>

<style>
table {
margin-top: 100px;  
margin-left: auto;
margin-right: auto;
font-family: "Lucida Sans Unicode", "Lucida Grande", Sans-Serif;
font-size: 14px;
border-radius: 10px;
border-spacing: 0;
text-align: center;
}
th {
background: #BCEBDD;
color: white;
text-shadow: 0 1px 1px #2D2020;
padding: 10px 20px;
}
th, td {
border-style: solid;
border-width: 0 1px 1px 0;
border-color: white;
}
th:first-child, td:first-child {
text-align: left;
}
th:first-child {
border-top-left-radius: 10px;
}
th:last-child {
border-top-right-radius: 10px;
border-right: none;
}
td {
padding: 10px 20px;
background: #F8E391;
}
tr:last-child td:first-child {
border-radius: 0 0 0 10px;
}
tr:last-child td:last-child {
border-radius: 0 0 10px 0;
}
tr td:last-child {
border-right: none;
}
.navigator{
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  margin-top: 20px;
}
.page{
  padding: 8px;
  margin-right: 5px;
  border: solid 1px black;
}
.page:hover{
  background: #F8E391;
  cursor: pointer;
  color: white;
}
.page_activ{
  background: #F8E391;
}


a.green{
border-radius: 4px;
color: #fff;
width:100px;
text-align: center;
font-family: Arial, Helvetica, sans-serif;
font-size: 14px;
padding: 8px 16px;
margin: 20px auto;
text-decoration: none;
    
}
a.green {
background-color: rgb( 43, 153, 91 );
border: 1px solid rgb( 33, 126, 74 );
}
        
a.green:hover {
background-color: rgb( 75, 183, 141 );
}
</style>
