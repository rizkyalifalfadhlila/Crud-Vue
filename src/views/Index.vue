<template>
<div class= "container mt-5">
    <div class="row">
        <div class="col-md-12">
            <div class="card border-0 rounded shadow">
                <div class="card-body">
                    <h4>DATA AKUN</h4>
                    <hr>
                    <router-link :to="{name : 'post.create'}" class="btn btn-md btn-success">
                        Tambah Post</router-link>
                        <table class="table table-striped table-bordered mt-4">
                            <thead class="thead-dark">
                                <tr>
                                <th scoppe="col">Username</th>
                                <th scoppe="col">Password</th>
                                <th scoppe="col">Options</th>
                                </tr>
                            </thead>
                            <tbody>
                                <tr v-for="(post, index) in posts" :key="index">
                                    <td>{{ post.username }}</td>
                                    <td>{{ post.password }}</td>
                                    <td class="text-center">
                                        <router-link :to="{name: 'post.edit', params: {id: post.id}}"
                                        class="btn btn-sm btn-primary mr-1">Edit</router-link>
                                        <button @click.prevent="postDelete(post.id)"
                                        class="btn btn-sm btn-danger mr-1">Hapus</button>
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
<script>
import axios from 'axios'
import { onMounted, ref} from 'vue'

export default {
    setup() {
        let posts = ref([])
        onMounted(() => {
            axios.get('http://localhost/uastekweb/public/api/akun')
            .then(respon => {
                const res = respon.data;
                const tmp  = {};

                if (res.success == true){
                    const data = res.data;
                    data.forEach((item) => {
                        tmp [item.id]=item;
                    });
                }
                console.log(tmp)
                posts.value = tmp;

            }).catch((error) => {
                console.log(error.response.data);
            });
        });
function postDelete(id) {
    axios
    .delete(`http://localhost/uastekweb/public/api/akun/${id}`)
    .then((response)=>{
        alert(id);
        if(response.data.success == true){
            delete posts.value[id];
        }
    })
    .catch((error) => {
        console.log(error.response.data);
    });
}

    return {
        posts,
        postDelete,
    };
    },
    };

</script>

<style>
body{
    background: lightgray;
}
</style>