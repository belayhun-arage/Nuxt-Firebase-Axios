<template>
  <div class="absolute top-0 right-0 left-0 bottom-20 flex flex-col justify-center items-center">
    <div id="receipt"  class="bg-white w-1/4 rounded-lg text-gray-800 font-semibold border border-gray-400">
      <div class="flex flex-col p-5 space-y-5 justify-center items-center">
        <div class="flex flex-col space-y-3">
          <div  class="text-lg text-center md:text-xl text-black">***POS SALES SLIP***</div>
          <div class="flex flex-col space-y-1">
              <div class="flex flex-row justify-between">
                <div >MERCHANT NO:</div> 
              </div>
              <div class="flex flex-row justify-between">
                <div >TERMINAL NO :</div> 
              </div>
              <div class="flex justify-center">Berhan Bank MPOS</div>
              <div>****************************************</div>
              <div class="flex flex-row justify-between">
                <div >Date :</div> 
              </div>
              <div class="flex flex-row justify-between">
                <div >Time :</div> 
              </div>
              
              <div class="flex flex-row justify-between">
                <div >Total :</div> 
              </div>
              <div class="flex flex-col py-1 space-y-1">
                <div>Card Holder Signature : </div>
                <div  class="flex w-40 h-20 bg-gray  self-center items-center justify-center border-4 rounded">
                    <img class="h-20" :src="'data:image/*;base64,' + srcimg" v-if="srcimg"/>
                    <div v-else>Loading...</div>
                </div>
              </div>
          </div>
        </div>
        <div >Powered by Belayhun Arage</div>
      </div>
    </div>
    <div class="flex flex-row space-x-32 p-5">
      <div>
          <button @click="goBack()" class="">
            <div class="bg-gray-300 hover:bg-gray-100 px-5 py-2 border border-gray-400 rounded shadow">
              <svg height="16px" version="1.1" viewBox="0 0 16 16" width="16px" xmlns="http://www.w3.org/2000/svg" xmlns:sketch="http://www.bohemiancoding.com/sketch/ns" xmlns:xlink="http://www.w3.org/1999/xlink"><title/><desc/><defs/><g fill="none" fill-rule="evenodd" id="Page-1" stroke="none" stroke-width="1"><g fill="#000000" id="Core" transform="translate(-424.000000, -4.000000)"><g id="arrow-back" transform="translate(424.000000, 4.000000)"><path d="M16,7 L3.8,7 L9.4,1.4 L8,0 L0,8 L8,16 L9.4,14.6 L3.8,9 L16,9 L16,7 L16,7 Z" id="Shape"/></g></g></g></svg>
            </div>
          </button>
          
      </div>
      <div class="bg-gray-300">
          <button @click="printDiv('receipt')" class=" hover:bg-gray-100 text-gray-800 font-semibold py-1 px-2 border border-gray-400 rounded shadow">
              Print
          </button>
      </div>
  </div>
</div>
</template>

<script>

import { getStorage, ref, getDownloadURL } from "firebase/storage";
export default {
  name: 'IndexPage',
  data(){
    return{
      srcimg:'',
    }
  },
  created(){
    const storage = getStorage();
    const starsRef = ref(storage, 'images/1670319341794')
    getDownloadURL(starsRef).then((url)=>{
        this.$axios.get(url).then((response)=>{
          this.srcimg=response.data
        })
    })
  },

  methods:{ 
    printDiv(divName){
			var printContents = document.getElementById(divName).innerHTML;
			var originalContents = document.body.innerHTML;

			document.body.innerHTML = printContents;

			window.print();

			document.body.innerHTML = originalContents;

		},
    PrintElem(){
        const MyElement=document.getElementById('receipt').innerHTML;
        const filename = "Report.txt";
        const element = document.createElement('a');
            
        element.setAttribute('href', 'data:text/plain;charset=utf-8,' + encodeURI(MyElement));
        element.setAttribute('download', filename);
        element.style.display = 'none';
        element.target = '_blank';
        document.body.appendChild(element);
        this.convertToPlain(element)
        element.click();
        document.body.removeChild(element);
    },
  }
}
</script>
