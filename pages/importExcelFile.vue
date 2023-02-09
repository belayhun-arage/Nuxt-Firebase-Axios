<template>
    <div class="pb-5">
        <label for="img" class="py-1 px-2 border-2 rounded text-lg">Import Excel File</label>
        <input type="file" id="img" style="display:none" @change="onChange">
      </div>
</template>

<script>
import * as XLSX from 'xlsx'
export default{
    data(){
        return{
            XLSX
        }
    },
    methods:{
        onChange(event) {
            this.file = event.target.files ? event.target.files[0] : null;
            if (this.file) {
                const reader = new FileReader();
                reader.onload = (e) => {
                /* Parse data */
                const bstr = e.target.result;
                
                const wb = XLSX.read(bstr, { type: 'binary' });
                
                /* Get first worksheet */
                const wsname = wb.SheetNames[0];
                const ws = wb.Sheets[wsname];
                /* Convert array of arrays */
                const data = XLSX.utils.sheet_to_json(ws, { header: 1 });
                console.log("the data is "+ data)
                }
                reader.readAsBinaryString(this.file);
            }
        },
    }
}
</script>