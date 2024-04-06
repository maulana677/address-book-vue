<script setup>
//import ref and onMounted
import { ref, onMounted } from "vue";

//import api
import api from "../../api";

//define state
const contacts = ref([]);

//method fetchDataPosts
const fetchDataContacts = async () => {
  //fetch data
  await api
    .get("/api/contact")

    .then((response) => {
      contacts.value = response.data.data;
    });
};

onMounted(() => {
  //memanggil method "fetchDataPosts"
  fetchDataContacts();
});

const deletePost = async (id) => {
  //hapus post with API
  await api.delete(`/api/contact/${id}`).then(() => {
    //memanggil method "fetchDataPosts"
    fetchDataContacts();
  });
};
</script>

<template>
  <div class="container mt-5 mb-5">
    <div class="row">
      <div class="col-md-12">
        <router-link
          :to="{ name: 'contacts.create' }"
          class="btn btn-md btn-success rounded shadow border-0 mb-3"
          >ADD NEW POST</router-link
        >

        <div class="col-md-6">
          <input
            type="text"
            class="form-control mb-3"
            placeholder="Cari..."
            v-model="search"
          />
        </div>

        <div class="card border-0 rounded shadow">
          <div class="card-body">
            <table class="table table-bordered">
              <thead class="bg-dark text-white">
                <tr>
                  <th scope="col">Image</th>
                  <th scope="col">Nama</th>
                  <th scope="col">No Telepon</th>
                  <th scope="col">Alamat</th>
                  <th scope="col" style="width: 15%">Aksi</th>
                </tr>
              </thead>
              <tbody>
                <tr v-if="contacts.length == 0">
                  <td colspan="5" class="text-center">
                    <div class="alert alert-danger mb-0">
                      Data Belum Tersedia!
                    </div>
                  </td>
                </tr>
                <tr v-else v-for="(contact, index) in contacts" :key="index">
                  <td class="text-center">
                    <img :src="contact.image" width="200" class="rounded-3" />
                  </td>
                  <td>{{ contact.nama }}</td>
                  <td>{{ contact.no_telp }}</td>
                  <td>{{ contact.alamat }}</td>
                  <td class="text-center">
                    <router-link
                      :to="{
                        name: 'contacts.edit',
                        params: { id: contact.id },
                      }"
                      class="btn btn-sm btn-primary rounded-sm shadow border-0 me-2"
                      >EDIT</router-link
                    >
                    <button
                      @click.prevent="deletePost(contact.id)"
                      class="btn btn-sm btn-danger rounded-sm shadow border-0"
                    >
                      DELETE
                    </button>
                  </td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>