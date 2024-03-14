<template>
  <h3>History</h3>
  <ul id="list" class="list">
    <li
      v-for="transaction in transactions"
      :key="transaction.id"
      :class="transaction.amount < 0 ? 'minus' : 'plus'"
    >
      {{ transaction.text }} <span>${{ transaction.amount }}</span
      ><button class="delete-btn" @click="deleteTransaction(transaction.id)">x</button>
    </li>
  </ul>
</template>

<script setup>
const emit = defineEmits(['transactionDeleted']);
import { defineProps } from 'vue';
const props = defineProps({
  transactions: {
    type: Array,
    required: true,
  },
});

const  deleteTransaction= (id)=>{
emit('transactionDeleted',id) ;
}
</script>

<!-- <script setup>
  const transactions = [
      {id:1, text:"flower", amount:-19.90},
      {id:1, text:"salary", amount:4009.90},
      {id:1, text:"Camera", amount:-200.90},
      {id:1, text:"Books", amount:99.90},

    ];

</setup> -->

<!-- <script>
// options API
// export default{
//   data(){
//     return{
//     transactions:[
//       {id:1, text:"flower", amount:-19.90},
//       {id:1, text:"salary", amount:4009.90},
//       {id:1, text:"Camera", amount:-200.90},
//       {id:1, text:"Books", amount:99.90},

//     ],
//   };
//   },
// };
// composition API

// two way to use composition API

// export default{
// setup() {
//   const transactions= [
//       {id:1, text:"flower", amount:-19.90},
//       {id:1, text:"salary", amount:4009.90},
//       {id:1, text:"Camera", amount:-200.90},
//       {id:1, text:"Books", amount:99.90},

//     ],
// if we want to create a function

// myfunction (){
//   what ever we want to do
// }
//     return{
//       transactions,
//  myfunction

//     };
//   },
// }

//</script> -->
