<template>
  <div class="container">
    <table class="table rounded">
  <thead class="">
    <tr>
      <th scope="col">Id</th>
      <th scope="col">Name</th>
      <th scope="col">Cost</th>
      <th scope="col">Address</th>
      <th>Edit</th>
      <th>Delete</th>
    </tr>
  </thead>
  <tbody>
    <tr v-for="(item, index) in data" :key="index" class="item">
      <th scope="row">{{ item.id }}</th>
      <td>{{item.name_uz}}</td>
      <td>{{item.cost}}</td>
      <td>{{item.address}}</td>
      <td><button class="btn btn-primary" @click="passUpdatePage(item?.id)">Edit</button></td>
      <td><button class="btn btn-danger" @click="deleteItem(item?.id)">Delete</button></td>
    </tr>
  </tbody>
</table>
    
    
  </div>
</template>

<script setup>
import { onMounted, ref } from 'vue'
import axios from 'axios'
import { useRouter } from 'vue-router'

const router = useRouter()

const data = ref([])

function fetchData() {
  axios
    .get('http://94.158.54.194:9092/api/product')
    .then((res) => {
      data.value = res.data
    })
    .catch((err) => {
      console.log(err)
    })
}

function deleteItem(id) {
  console.log(id, 'id')
  axios
    .delete(`http://94.158.54.194:9092/api/product/${id}`)
    .then((res) => {
      console.log(res)
      fetchData()
    })
    .catch((err) => {
      console.log(err)
    })
}

function passUpdatePage(id) {
  const singleData = data.value.find((el) => el.id === id)
  router.push({
    path: `update/${id}`,
    query: {
      name: singleData?.name_uz,
      cost: singleData?.cost,
      address: singleData?.address
    }
  })
}

onMounted(() => {
  fetchData()
})
</script>

<style>
td{
  font-size: 20px;
  text-align: start;
  padding: 10px ;
  text-align: center;
}
th{
  text-transform: uppercase;
  text-align: center;
  
}

</style>
