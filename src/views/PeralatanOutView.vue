<template>
  <div class="card p-3 shadow">
    <h5>Peralatan Diluar</h5>
    <br />
    <button type="button" style="width: 120px" class="btn btn-primary mb-3" data-bs-toggle="modal" data-bs-target="#modalmasuk">
      <i class="fa-solid fa-file-circle-plus me-1"></i>Add Data
    </button>

    <div class="modal fade" id="modalmasuk" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
      <div class="modal-dialog modal-lg modal-dialog-centered">
        <div class="modal-content">
          <form @submit.prevent="submitData">
            <div class="modal-header">
              <h1 class="modal-title fs-5" id="exampleModalLabel">Tambah Data</h1>
              <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
            </div>
            <div class="modal-body">
              <div class="row">
                <div class="col">
                  <div class="mb-3">
                    <label class="form-label">Lokasi</label>
                    <input v-model="form.lokasi" type="text" class="form-control" />
                  </div>
                  <div class="mb-3">
                    <label class="form-label">Jenis Barang</label>
                    <input v-model="form.jenis_barang" type="text" class="form-control" />
                  </div>
                  <div class="mb-3">
                    <label class="form-label">Merk</label>
                    <input v-model="form.merk" type="text" class="form-control" />
                  </div>
                  <div class="mb-3">
                    <label class="form-label">Jumlah</label>
                    <input v-model="form.jumlah" type="text" class="form-control" />
                  </div>
                </div>
                <div class="col">
                  <div class="mb-3">
                    <label class="form-label">Satuan</label>
                    <input v-model="form.satuan" type="text" class="form-control" />
                  </div>
                  <div class="mb-3">
                    <label class="form-label">Kondisi</label>
                    <input v-model="form.kondisi" type="text" class="form-control" />
                  </div>
                  <div class="mb-3">
                    <label class="form-label">Tanggal</label>
                    <input v-model="form.tanggal" type="date" class="form-control" />
                  </div>
                  <div class="mb-3">
                    <label class="form-label">Dokumen</label>
                    <input v-model="form.dokumen" type="text" class="form-control" />
                  </div>
                </div>
              </div>
            </div>
            <div class="modal-footer">
              <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Close</button>
              <button type="submit" class="btn btn-primary">Submit</button>
            </div>
          </form>
        </div>
      </div>
    </div>

    <table class="table table-striped table-bordered">
      <thead>
        <tr>
          <th scope="col">No</th>
          <th scope="col">Lokasi</th>
          <th scope="col">Jenis Barang</th>
          <th scope="col">Merk</th>
          <th scope="col">Jumlah</th>
          <th scope="col">Satuan</th>
          <th scope="col">Kondisi</th>
          <th scope="col">Tanggal</th>
          <th scope="col">Dokumen</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(item, index) in peralatanDiluarData" :key="item.id">
          <td>{{ index + 1 }}</td>
          <td>{{ item.lokasi }}</td>
          <td>{{ item.jenis_barang }}</td>
          <td>{{ item.merk }}</td>
          <td>{{ item.jumlah }}</td>
          <td>{{ item.satuan }}</td>
          <td>{{ item.kondisi }}</td>
          <td>{{ item.tanggal }}</td>
          <td>{{ item.dokumen }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: "PeralatanOutView",
  data() {
    return {
      form: {
        lokasi: '',
        jenis_barang: '',
        merk: '',
        jumlah: '',
        satuan: '',
        kondisi: '',
        tanggal: '',
        dokumen: ''
      },
      peralatanDiluarData: []
    };
  },
  methods: {
    async submitData() {
      try {
        await axios.post('http://localhost:8000/api/peralatan_diluar/add', this.form);
        this.fetchData();
        this.resetForm();
        this.$refs.closeModalButton.click(); // Assuming there is a ref on the close button to close the modal
      } catch (error) {
        console.error('Error submitting data:', error);
      }
    },
    async fetchData() {
      try {
        const response = await axios.get('http://localhost:8000/api/peralatan_diluar');
        this.peralatanDiluarData = response.data;
      } catch (error) {
        console.error('Error fetching data:', error);
      }
    },
    resetForm() {
      this.form = {
        lokasi: '',
        jenisBarang: '',
        merk: '',
        jumlah: '',
        satuan: '',
        kondisi: '',
        tanggal: '',
        dokumen: ''
      };
    }
  },
  mounted() {
    this.fetchData();
  }
};
</script>
