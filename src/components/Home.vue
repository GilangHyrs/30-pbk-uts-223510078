<template>
    <div class="home-container">
      <div class="home-content">
        <h1 class="home-title">Eksplorasi Dunia Bersama Web ini</h1>
        <p class="home-description">Temukan Keindahan Dunia Dengan Web ini. Mari Jelajahi Berbagai Konten Menarik, mulai Menentukan Tugas-Tugas Harian, Cerita Tentang Kegiatan Anda,Galeri Foto Yang Mengagumkan.</p>
        <router-link to="/about" class="btn" @click="handleAboutClick">Tentang Kami</router-link>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    methods: {
      handleAboutClick() {
        this.$router.push('/about');
      }
    }
  }
  </script>
  
  <style>
  .home-container {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
    background-image: url('https://fiverr-res.cloudinary.com/images/q_auto,f_auto/gigs/129325364/original/afaddcb9d7dfaaf5bff7ef04101935814665ac16/design-an-attractive-background-for-your-website.png');
    background-size: cover;
    background-position: center;
  }
  
  .home-content {
    text-align: center;
  }
  
  .home-title {
    font-size: 36px;
    font-weight: bold;
    color: #fff;
    margin-bottom: 20px;
    text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.5); 
  }
  
  .home-description {
    font-size: 18px;
    color: #fff;
    margin-bottom: 30px;
  }
  
  .btn {
    padding: 10px 20px;
    background-color: #007bff;
    color: white;
    text-decoration: none;
    border-radius: 5px;
    transition: background-color 0.3s;
  }
  
  .btn:hover {
    background-color: #0056b3;
  }
  </style>
  