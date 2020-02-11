<template>
    <div class="container">
        <div class="row justify-content-center">
            <div class="col-md-8">
                <br>

                <div class="card">
                    <div align="center" class="card-header">
                    {{ judul}}
                    </div>

                    <div v-if="!loading" class="card-body">

                        <input
                            class="form-control"  
                            v-model="katakunci" 
                            type="search"
                            placeholder="Masukan kata kunci dan enter"
                            @change="jalankanPencarian">
                            <br>

                        <button
                        class="btn btn-primary "
                        @click="aksesApi">
                        Refresh Data
                        </button>

                        <hr>

                        <table v-if="!error" class="table table-bordered "> 
                            <tr style="background: #f6993f;" >
                                <td>Nama </td>
                                <td>Jenis Kelamin</td>
                                <td>Tanggal Dilahirkan</td>
                            </tr>
                            <tr v-for="item in Data.data" :key="item.id">
                                <td> {{ item.nama }}</td>
                                <td> {{ item.jk }}</td>
                                <td> {{ item.created_at }}</td>                                
                            </tr>
                        </table>

                        <pagination :data="Data" @pagination-change-page="aksesApi">
                            <span slot="prev-nav">&lt; Previous</span>
                            <span slot="next-nav">Next &gt;</span>
                        </pagination>

                        <div class="alert alert-danger" v-if="error" > {{ error }}</div>

                    </div>

                    
                    <div v-if="loading" class="card-body" >Loading . . .</div>

                </div>
                <notifications group="foo"/>
            </div>
             <vue-progress-bar></vue-progress-bar>
        </div>
    </div>
</template>

<script>
    export default {
        data(){
            return {
                judul: 'Data Siswa',
                Data: {},
                error : null,
                loading : false,
                katakunci :''
            }
        },

        mounted() {
            //console.log('Component mounted.')
            this.aksesApi();
            this.jalankanPencarian();
        },

        methods:{
            aksesApi(page = 1) {
                this.loading = true
                this.$Progress.start()

                const params = {
                    page : page
                }

                axios.get('/testapi', )
                .then(response => {
                    this.Data = response.data
                    this.loading = false
                    this.$Progress.finish()
                    this.$notify({
                        type: 'bg-success',
                        group: 'foo',
                        title: 'sukses',
                        text: 'Berhasil HOREE!'
                    });
                })
                .catch(error => {
                    this.error = error
                    this.loading = false
                    this.$Progress.fail()
                    this.$notify({
                        type: 'bg-danger',
                        group: 'foo',
                        title: 'warning',
                        text: 'ERROR!'
                    });
            })
            },

            jalankanPencarian(){

            }

        }


    }
</script>
