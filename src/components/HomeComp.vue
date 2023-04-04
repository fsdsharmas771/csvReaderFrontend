<template>
<div class="home-main">
    <div class="form-file-list">
        <div class="form">
            <input type="file" @change="selectFile">
            <button v-on:click="uploadFile">Upload</button>
        </div>
        <div class="document-list">
            <table border="1px" class="document-table">
                <tr>
                    <td>Name</td>
                    <td>Data</td>
                    <td>Action</td>
                </tr>
                <tr v-for="item in documents" :key="item.id">
                    <td>{{item.fileName}}</td>
                    <td><button v-on:click="loadData(item._id)">Click</button></td>
                    <td><button v-on:click="deleteFile(item._id)">Delete</button></td>
                </tr>
            </table>
        </div>
    </div>
    <aside>
        <h5>A:Hours_worked_per_week_in_other_jobs</h5>
        <h5>B:Employed_census_usually_resident_population_count_aged_15_years_and_over</h5>
        <table border="1px" class="document-data-table">
            <tr>
                <td>Code</td>
                <td>A</td>
                <td>B</td>
            </tr>
            <tr v-for="item in documentData" :key="item.id">
                <td>{{item.Code}}</td>
                <td>{{item.Hours_worked_per_week_in_other_jobs}}</td>
                <td>{{item.Employed_census_usually_resident_population_count_aged_15_years_and_over}}</td>
            </tr>
        </table>
    </aside>
</div>
</template>

<script>
import axios from 'axios'

export default {
    name: 'HomeComp',
    data() {
        return {
            selectedFile: null,
            documents: [],
            documentData: []
        }
    },
    methods: {
        selectFile(event) {
            this.selectedFile = event.target.files[0]
        },
        async uploadFile() {
            console.log(this.selectedFile)
            let result = await axios.post("https://csvreaderbackend.onrender.com/v1/csv/upload", {
                file0: this.selectedFile
            }, {headers: {
                'Content-Type': 'multipart/form-data'
            }});
            if(result.status==200){
                this.loadList();
            }
            
        },
        async loadList() {
            let result = await axios.get("https://csvreaderbackend.onrender.com/v1/csv/list/files");
            this.documents = result.data.data;
        },
        async loadData(id) {
            let tableId = id;
            let result = await axios.get(`https://csvreaderbackend.onrender.com/v1/csv/list/rows/${tableId}`);
            console.log(result);
            this.documentData = result.data.data;
        },
        async deleteFile(id) {
            let result = await axios.delete(`https://csvreaderbackend.onrender.com/v1/csv/${id}`);
            if (result.status == 200) {
                this.loadList();
            }
        }
    },
    mounted() {
        this.loadList();
    }
}
</script>

<style>
.home-main {
    height: auto;
    min-height: 85vh;
    widows: 100%;
    display: flex;
    justify-content: space-around;

}

.form-file-list {
    border: solid 2px green;
    width: 45%;
    margin: 5px;
}

.form {
    margin-top: 5px;
    border-bottom: 2px solid green;
    margin: 10px;
    height: 10%;
    display: flex;
    justify-content: center;
    align-items: center;
}

.document-list {
    height: 80%;
    margin: 10px;
}

.document-table {
    margin-left: 10px;
}

.document-table td {
    width: 200px;
}

aside {
    width: 45%;
    margin: 5px;
    display: flex;
    flex-direction: column;
    align-items: center;
}

.document-data-table td {
    width: 180px;
}
</style>
