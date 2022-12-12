<template>
    <div>
       <NavBar />
       <div class="header">
 
          <img src="@/assests/image/bannerImage.jpg" alt="" />
 
          <div class="text-container">
             <div class="text justify-center flex">
                <div class="">                  
                </div>
                
                <h1 class="pt-20">
                    <span>Newest</span>
                    <span>&</span>
                    <span>Top 3 Articles</span>
                </h1>
                
             </div>
          </div>
       </div>
    </div>
 </template>
 
 <script>

 import NavBar from '@/components/nav_bar.vue'
 export default {
    components: {      
       NavBar
    },
 }
 </script>
 
 
 <style lang="scss" scoped>
 .header {
    height: 400px;
    position: relative;
 
    @media (min-width: 750px) {
       height: 280px;
    }
 
    &::after {
       content: '';
       position: absolute;
       top: 0;
       left: 0;
       height: 100%;
       width: 100%;
    //    background-color: rgba(0, 0, 0, 0.6);
    }
 
    img {
       width: 100%;
       height: 100%;
       object-fit: cover;
       filter: blur(4px);
    }
 
    .text-container {
       z-index: 99;
       position: absolute;
       top: 0;
       margin: 0;
       width: 100%;
       height: 100%;
       display: flex;
       flex-direction: column;
       justify-content: center;
 
       .text {
          padding: 0 0px;
          width: 100%;
          max-width: 1400px;
          margin: 0 auto;
          text-shadow: 2px 2px 12px rgb(239 68 68);
       }
 
       h1 {
          color: #fff;
          font-size: 64px;
          font-weight: 200;
          margin-bottom: 8px;
 
          @media (min-width: 750px) {
             font-size: 84px;
          }
 
          span {
             font-weight: 500;
          }
       }
 
       .view {
          font-size: 20px;
          align-self: flex-start;
       }
    }
 }
 </style>