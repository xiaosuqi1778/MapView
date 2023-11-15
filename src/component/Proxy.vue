<script setup>
const axios = require('axios');

async function sendRequest() {
  try {
    const response = await axios.post('http://localhost:3000/proxy', {
      address: '北京市朝阳区阜通东大街6号', // 这里替换为你的具体地址
      city: '北京',
      output: 'json'
    });

    console.log(response.data);
  } catch (error) {
    console.error(error);
  }
}

sendRequest();

</script>